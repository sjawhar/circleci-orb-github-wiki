## Purpose

This CircleCI orb automates the building and deployment of documentation to the wiki repo available every GitHub repository.

What? You didn't know that the wiki tab was controlled by a git repo? Well, it's true! And this orb will help you make the most of it.

## Jobs
### build-and-push
Populate sidebar files with page header links and push to wiki repo.

**Parameters**
* `commit-user-name`: User name with which wiki changes should be commited. Default: `CircleCI`
* `commit-user-email`: User email with which wiki changes should be commited
* `sidebar-placeholder`: String in sidebar files to replace with page header links. Default: `{{SIDEBAR_POPULATE}}`
* `ssh-key-env-var-name`: Name of the environment variable containing the fingerprint of the SSH key with push access to your repo. Default: `GITHUB_WIKI_SSH`
* `wiki-folder-path`: Path to directory containing wiki files. Default: `docs`

## Commands
### populate-sidebars
Populate sidebar files with page header links.

**Parameters**
* `sidebar-placeholder`

### push-to-wiki-repo
Copy files to wiki repo and push.

**Parameters**
* `commit-user-name`
* `commit-user-email`
* `ssh-key-env-var-name`

## Example
```yaml
version: 2.1

orbs:
  github-wiki: sjawhar/github-wiki@0.1.2

workflows:
  deploy-wiki:
    jobs:
      - github-wiki/build-and-push:
          commit-user-email: circleci+orb-github-wiki@thecybermonk.com
          filters:
            branches:
              only: master
```
