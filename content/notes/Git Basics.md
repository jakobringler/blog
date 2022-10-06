---
title: "Git Basics"
draft: false
tags:
---

## Concept

## Workflow
1. pull latest
2. create new branch
3. do work
4. rebase against main/master
	1. resolve conflicts
5. push to remote
6. raise pull request
7. discuss
8. merge

## Setup
### Initializing Git Repo:
// create git repo in current dir:

```bash
git init . 
```

// get version

```bash
git --version
```

### Staging Files
// add files to be tracked by git

```bash
git add filename
```

This marks the file and tells git to pay attention to it when commiting to the commit history

// add all files from this directory and downwards

```bash
git add .
```

// stage all files in repo (also upwards)

```bash
git add -A
```

// unstage files

```bash
git rm -r --cached filename

git rm -r --cached .
```

### Status
// get insight into which files are staged, commited or untracked

```bash
git status
```

### Configuration
// configure user info etc.

```bash
git config --global user.name "[name]"

git config --global user.email "[email address]"

git config --global color.ui auto
```

## Commits
Like a save point for all staged files

```bash
git commit -m "message to describe what is being commited"
```

// get overview of commits 

```bash 
git log
```

// compact layout

```bash
git log --oneline
```

// show specifics of a commit

copy COMMITHASH from log and dod

```bash
git show COMMITHASH
```

// show changes since last commit

```bash
git diff
```

// add messages after commit

```bash
git commit --amend -m "message"
```

## Branches

> Branches are an important part of working with Git. Any commits you make will be made on the branch you’re currently “checked out” to. Use `git status` to see which branch that is.

// get current branch

```bash
git branch
```

// show all branches

```bash
git branch -a
```

// show remote branches

```bash
git branch -r
```

// rename branch to main

```bash
git branch -M main
```

// create branch

```bash
git branch branchname
```

// switch branch

```bash
git checkout branchname
```

// switch and create branch

```bash
git checkout -b branchname
```

// switch to main

```bash
git checkout -
```

// delete branch

```bash
git branch -d branchname
```

## Push
// push the repository to the remote github repo

```bash
git remote add origin git@github.com:...
```

```bash
git push -u origin main
```

// push new branch to remote

```bash
git push --set-upstream origin branchname
```

or

```bash
git push -u origin branchname 
```

## GitHub
### Configure SSH Keys
If you can't push to your remote repo you might have to configure an ssh access

-> [GitHub SSH Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### Clone from Remote
// download / mirror remote repo to local machine

```bash
git clone https://github.com/username/reponame.git
```

## Pull
// download changes from remote server 

```
git pull
```

// get from specific branch 

```bash
git pull origin main
```

## Pull Requests

## Merge
// merge branch to current branch (main)

```bash
git merge branchname
```

## Rebase

## .gitignore file

> Sometimes it may be a good idea to exclude files from being tracked with Git. This is typically done in a special file named `.gitignore`. You can find helpful templates for `.gitignore` files at [github.com/github/gitignore](https://github.com/github/gitignore).

## Submodules
to be able to use git repos inside other git repos use submodules:

// install a git repo as a submodule into another repo

```bash
git submodule add https://github.com/username/reponame
```

[Submodules - Git Book](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

---

sources / further reading:
- [Git Cheat Sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
- [Git and GitHub Tutorial For Beginners | Full Course [2021] [NEW]](https://www.youtube.com/watch?v=3fUbBnN_H2c)
- [Git MERGE vs REBASE](https://www.youtube.com/watch?v=CRlGDDprdOQ)

