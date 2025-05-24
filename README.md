Nuqs
==========================================================================================================

**NOTE: THIS IS A REC-SPECIFIC FORK OF [47ng/nuqs](https://github.com/47ng/nuqs).** The purpose of this fork is to
vendor this library into the [rec monorepo](https://github.com/rec-eng/monorepo) and make it compatible with our current
setup there.

### Background

* The library as published is ESM-only, which is stupid and causes problems with our current monoerepo setup.
* These problems were really hard to resolve and so eventually I gave up and decided to just vendor the library and
  convert it to CJS.
* However, the upstream repo is also a monorepo, so I tore out all that infrastructure and just published the nuqs
  package by itself.
* This branch of the repo is now a copy of the `packages/nuqs` folder in the upstream repo but with compile settings
  set to use CJS instead of ESM. It is then cloned into the monorepo (at `vendor/nuqs`) and used as a dependency there.

### How to Work With This

This branch was forked from the v2.4.3 tag of the upstream repo. The best way to incorporate new changes will likely be
to rebase onto later version as they come out. Here's how you'd do that from scratch:

```sh
# Clone and enter this repo (should already be on the `rec-commonjs` branch)
git clone https://github.com/rec-eng/nuqs.git
cd nuqs

# Add the upstream repo as a remote
git remote add upstream https://github.com/47ng/nuqs.git

# Fetch the latest changes from upstream
git fetch upstream

# Rebase your local changes onto the latest upstream changes
git rebase upstream/v2.9.1 # or whatever version you want

# Push the changes back up to our fork
git push
```

Then in the monorepo:

```sh
# Switch to main and pull the latest
git checkout main && git pull

# Make sure the nuqs submodule is initialized
git submodule update --init

# Switch to the rec-commonjs branch and pull the latest
(cd vendor/nuqs && git checkout rec-commonjs && git pull)

# Commit the changes and PR
git add ./ && gt create -m '[CHORE]: Update nuqs' && gt submit
```
