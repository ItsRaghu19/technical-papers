# Git Cheat Sheet


## Starting a project

| Command | What it does |
|---|---|
| `git init` | Initialize a repository |
| `git clone <url>` | Clone an existing repository |
| `git status` | Show the current working tree status |
| `git config --global user.name "Name"` | Set your Git username |
| `git config --global user.email "email"` | Set your Git email |

Set identity for just the current repository by omitting `--global` and running the command inside that repository.

## Basic Git workflow

```bash
git status
git add file.txt
git commit -m "Add file"
git push
```

| Command | What it does |
|---|---|
| `git add file.txt` | Stage a specific file |
| `git add .` | Stage changes in the current directory |
| `git commit -m "message"` | Create a commit |
| `git log --oneline` | View commit history |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |

## Branching and merging

| Command | What it does |
|---|---|
| `git branch` | List local branches |
| `git branch leaf` | Create a branch |
| `git switch leaf` | Switch to a branch |
| `git switch -c leaf` | Create and switch to a branch |
| `git merge leaf` | Merge a branch into the current branch |
| `git branch -d leaf` | Delete a merged branch |

## Remote repositories

| Command | What it does |
|---|---|
| `git remote -v` | List remote repositories |
| `git remote add origin <url>` | Add a remote |
| `git fetch` | Download remote updates |
| `git pull` | Fetch and integrate remote changes |
| `git push -u origin main` | Push a branch and set its upstream |

## Undoing changes

| Command | What it does |
|---|---|
| `git restore file.txt` | Discard unstaged changes to a file |
| `git restore --staged file.txt` | Unstage a file |
| `git commit --amend` | Edit the last commit |
| `git revert <commit>` | Create a new commit that undoes another commit |
| `git reset --soft HEAD~1` | Undo the last commit, keeping changes staged |
| `git reset --hard HEAD~1` | Undo the last commit and discard its changes |

**Caution:** `git reset --hard` can permanently discard uncommitted work.

## Practice: initialize, commit, branch, and merge

This follows the requested exercise using `master` as the main branch name.

```bash
mkdir git-practice
cd git-practice

git init
git branch -M master

# Add a.txt and make the first commit
touch a.txt
git add a.txt
git commit -m "Add a.txt"

# Create and switch to leaf
git switch -c leaf

# Add b.txt and make the second commit
touch b.txt
git add b.txt
git commit -m "Add b.txt"

# Return to master and merge leaf
git switch master
git merge leaf

# Verify
git log --oneline --graph --all
git status
```

If Git asks you to configure your identity, set `user.name` and `user.email` as shown in the setup section.

## Git workflow

```text
Working Directory
       |
    git add
       ↓
Staging Area
       |
   git commit
       ↓
Local Repository
       |
    git push
       ↓
Remote Repository
```

## Quick reference

- **Working tree:** files you are editing.
- **Staging area/index:** changes prepared for the next commit.
- **Commit:** a saved snapshot in the local repository.
- **Branch:** a movable reference to a commit.
- **Merge:** combines the histories of branches.
- **Remote:** a repository hosted elsewhere, such as GitHub or GitLab.
