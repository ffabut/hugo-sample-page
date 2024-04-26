# Hugo Sample Page

This is an sample/example/boilerplate repository showing minimalistic Hugo project with automated builds and deployments (in Github Actions) to hosting on GitHub Pages.
You can fork this repository and use it a starting point for your own website.
Or just copy the [.github/workflows/build.yml] to your existing Hugo project to automate its build and deployment.

## Description

### How repo was made
This is a boring stuff, just to explain the Hugo code in this repo.
There is nothing fancy, mostly just default hugo commands to init a Hugo project:

1. `hugo new site hugo-sample-project` to init Hugo project, `cd hugo-sample-project` to get inside the project directory
2. `hugo new theme default-theme` to init default Hugo template (otherwise it would not build)
3. added `theme = 'default-theme'` to hugo.toml to activate the theme
4. added file `.gitignore` with line `public` to ignore local builds in `public` directory (no need to keep this in git history)
5. that is all 99% of this repository

## Automation

### Why automation
This repository uses Github Actions to automate builds and deployments.
Actions are free for public repositories and there is quite some free runners time for private repos.
Using the Actions we can automate the build, which saves us need to run `hugo` or `hugo --minify` before every commit.

Automation is also practical for situations when you collaborate on one web with multiple people.
Ideally everybody creates a PR for their changes, PR is tested, accepted by others and it is merged to the main branch.
Without automation, one would need to pull the changes from main branch, build the page with `hugo --minify` and push the changes in `./public` directory back to repository.
Automation can detect new commit in `main` branch, it installs Hugo on virtual computer, runs the build and uploads the contents of `public` directory into `gh-pages` branch, from which website is being served by GitHub Pages.
This saves you from wasting your time and making mistakes!

### Workflow definition
Github Actions expects workflow files to be stored at `.github/workflows` directory.
This repo contains one file `.github/workflows/build.yml` which defines the workflow for build and deployment.
There is no special need to change this file, it is just a standard workflow file, you can add it into your project.
For more information take a look at comments in the file.
In short:
1. it runs on every push to `main` branch, and on every new commit in PR
2. it starts virtual machine with Ubuntu operating system
3. it installs Hugo
4. it clones the repository (main branch or the PR branch)
5. it runs `hugo --minify` command to build the website into `public` dir
6. it uploads the contents of `public` directory into `gh-pages` branch

Once action successfully run, you can see the results on the `gh-pages` branch: https://github.com/username/reponame/tree/gh-pages (replace username and reponame with appropriate values, for this repo: ffabut and hugo-sample-project -> https://github.com/ffabut/hugo-sample-page/tree/gh-pages).

### Setup on your side
You need to enable Github Pages on your repo and configure it.
1. go to https://github.com/username/reponame/settings/pages (for this repo it would be: https://github.com/ffabut/hugo-sample-page/settings/pages)
2. in section `Build and deployment`
3. set `Source` to `Deploy from branch`
4. set `Branch` to `gh-pages` and `/root`

We select `Deploy from branch` and branch `gh-pages` because it is more practical to keep the built files outside our main branch, it keeps our main branch more clean, we do not clutter it by deploying to `public` directory in main branch.

We select `/root` directory because as we use different branch than main, there is no need to put the build into a directory like `public`.
We can just server from the root of the branch. 
Github Action is configured in the same manner.

Once this is set, every push to `main` branch will trigger the build and deployment.
And after a while, changes should be online!
Congratz!
