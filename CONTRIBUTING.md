# Syncing with upstream

## Upstream copy

Upstream fork which this is based on can be found here:
https://github.com/giantswarm/falco-charts

### Chart repo

The subfolders in `helm/falco-app/charts` were directly pulled from upstream like this:

```bash
# Add remote and pull latest.
git remote add -f --no-tags upstream-copy git@github.com:giantswarm/falco-charts.git
git fetch upstream-copy master

# Add falco subfolder in charts.
git checkout upstream-copy/master
git subtree split -P falco -b temp-split-branch
git checkout master
git subtree add --squash -P helm/falco-app/charts/falco  temp-split-branch

# Add falco-exporter subfolder in charts.
git checkout upstream-copy/master
git subtree split -P falco-exporter -b temp-split-branch-exporter
git checkout master
git subtree add --squash -P helm/falco-app/charts/falco-exporter  temp-split-branch-exporter

# Add falco-sidekick subfolder in charts and rename from falcosidekick.
git checkout upstream-copy/master
git subtree split -P falcosidekick -b temp-split-branch-sidekick
git checkout master
git subtree add --squash -P helm/falco-app/charts/falco-sidekick  temp-split-branch-sidekick
```

## Workflows (not customized to this repo)

### I want to setup my local repos after they were already created for the first time

- to setup "upstream-copy":

  ```
  git clone git@github.com:giantswarm/grafana-helm-charts-upstream.git
  cd grafana-helm-charts-upstream
  git checkout upstream-main
  git remote add -f upstream https://github.com/grafana/helm-charts.git
  ```

- to setup "chart repo"

  ```
  git clone git@github.com:giantswarm/loki-app.git
  cd loki-app
  git remote add -f --no-tags upstream-copy git@github.com:giantswarm/grafana-helm-charts-upstream.git  # add remote
  ```

### I want to update to the latest version from upstream

Assuming you want to get to the state of `main` branch in `upstream-main`. If you want any other state,
replace `upstream/main` with any other branch or just tag: `v.X.Y.Z` (to see upstream tags, you need to
skip the `--skip-tags` flag, as explained [above in set up instructions](#chart-repo)).

1. In "upstream-copy" repo
  - checkout "upstream-main" branch
  - fetch changes from "upstream/main", merge them with "upstream-main"
  - checkout "main", merge "upstream-main" to it, push "master"
  - example commands:

  ```
  git checkout upstream-main
  git fetch upstream
  git merge upstream/main
  # create PR in github from upstream-main to main XOR run
  git checkout main
  git merge upstream-main
  git push
  ```

2. In "chart repo"
  - if the subtree is tracking the whole "upstream copy" repo

  ```
  git fetch upstream-copy main
  git subtree pull --prefix helm/loki-app upstream-copy main --squash
  ```

  - if the subtree is tracking a subdir of "upstream copy":

  ```
  git fetch upstream-copy main  # fetch the most recent state from "upstream copy/main"
  git checkout upstream-copy/main  # it's OK to be in detached head, we won't change anything
  git subtree split -P charts/loki-distributed -b temp-split-branch
  git checkout master
  git subtree merge --squash -P helm/loki temp-split-branch
  git push
  git branch -D temp-split-branch
  ```

### I want to send non-urgent patch for upstream

Do this if you want to submit a patch for "upstream", but you also want to wait it until it
is accepted by upstream (so, you'll get your patch applied and then get it from "upstream" someday):

- go to "upstream copy", update remote "upstream" and fetch changes into the "upstream-main" branch
- create a branch "my-feature" from "upstream-main"
- when ready, create a PR for "upstream"
- when PR is merged, remove local "my-feature" branch and update our dependencies as in [normal upstream update](#I-want-to-update-to-the-latest-version-from-upstream)

### I want to send urgent patch for upstream and use it already

Do this if you want to submit a patch for "upstream" and you need to use it right away, without waiting
for being accepted by upstream:

- go to "upstream copy", update remote "upstream" and fetch changes into the "upstream-main" branch
- create a branch "my-feature" from "upstream-main"
- when ready, create a PR (PR1) for "upstream"
- create another PR (PR2) to merge "my-feature" into "main"
- when PR2 is merged, update "chart repo" dependency on "upstream-copy/master" as in point 2 in [normal upstream update](#I-want-to-update-to-the-latest-version-from-upstream)
- when PR1 is merged, remove local "my-feature" branch and update our dependencies as in [normal upstream update](#I-want-to-update-to-the-latest-version-from-upstream)

### I want to make changes that I don't want to be ever sent to upstream

Do this if you want to make any Giant Swarm specific changes to the chart.
We have two options about where to do that and it's up to you to think where it makes the most
sense.

1. In the "chart repo" - this should be your default.

  - just do it - you can commit and change anything you want in the "subtree" catalog and your
    changes won't be lost when you update it.

2. In the "upstream copy" repo - makes sense for cases where multiple charts include some shared
   sub-chart and you want to patch it.

  - go to "upstream copy", checkout and update "main" branch
  - create a branch "my-feature" from "main"
  - when ready, create a PR from "my-feature" to "main"
  - when PR is merged, update "chart repo" dependency on "upstream-copy/master" as in point 2 in [normal upstream update](#I-want-to-update-to-the-latest-version-from-upstream)
