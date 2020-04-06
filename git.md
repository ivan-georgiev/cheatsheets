# Git commands

## Branches and remotes

```bash
# Create new branch from current branch
git checkout -b feature/new1

# Create new branch from origin master
git checkout -b feature/new1 origin/master

# Unset upstream of a branch 1
git branch --unset-upstream
# or
git config --unset branch.feature/new1.remote
git config --unset branch.feature/new1.merge

# Push local branch to origin when does now exist
git push -u origin feature/new1

# Link local branch with origin branch
git branch --set-upstream feature/new1 origin/feature/new1

# List local branches and tracked remotes
git branch -vv

# List tracked remote URL
git ls-remote --get-url
```
## Commits compare and change

```bash
# List commit difference between current branch and remote - both sides
git log --graph --left-right --oneline origin/master...HEAD
# or
git rev-list origin/master HEAD --not $(git merge-base --all origin/master HEAD)

# List commits present on local but not on remote
git log origin/master..HEAD --oneline
# or
git rev-list HEAD ^origin/master
# or same without merge commits
git log origin/master..HEAD --oneline --no-merges

# Git view commit relations between all branches
git log --graph --color --decorate --oneline --all
# or graphical 
gitk --all

# List all remote branches containing commit
git branch -r --contains HEAD~2

# Git get current commitId 
git rev-parse HEAD

# Git interactive rebase on last 4 commits, typically to squash or change commit messages.
git rebase -i HEAD~4

# Revert last pushed commit (new commit is generated)
git revert HEAD

# Revert last 3 pushed commits (new commit is generated)
git revert --no-commit HEAD~3..

# Revert last 3 non-pushed commits (commits are deleted). If pushed, --force must be used to push the changes.
git reset HEAD~3

# Get fork point from a branch
git merge-base --fork-point origin/master
```

## Files compare and change

```bash
# List files changed on current local branch compared with origin/master. Default upstream can be referred as '@{u}'
git --no-pager diff --name-status origin/master...HEAD

# List commits where file was changed
git log --follow ./path_to_check

# List files changed in a commit
git show HEAD --name-only
git show -n 2 --name-only

# Git add exec bit on a file
git update-index --chmod=+x myscript.sh

# Git list files
git ls-files -s

# Git checkout a file/folder from other branch
git checkout origin/master -- ./path_to_checkout

# Restore branch to last commit state
git reset --hard HEAD

# File Git SHA
git hash-object $file
cat $file | git hash-object --stdin
(echo -ne "blob `wc -c < $file`\0"; cat $file) | sha1sum
# file in index
git ls-files -s $file | awk '{print $2}'

```