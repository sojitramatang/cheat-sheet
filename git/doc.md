# Git Basic Commands

## Initialize Repository
```bash
git init
git clone [repository-url]  # Clone a repository
git clone -b [branch-name] [repository-url] # Clone a specific branch
```

## Set Remote Repository
```bash
git remote -v                                   # View remote repositories
git remote rm origin                            # Remove a remote repository
git remote rename origin [new-name]             # Rename a remote repository
git remote set-url origin [repository-url]      # Change the URL of a remote repository
git remote add origin [repository-url]          # Add a remote repository
```



## Check Repository Status
```bash
git status
```

## Stage Changes
```bash
git add [file-name]  # Stage specific file
git add .            # Stage all changes
```

## Commit Changes
```bash
git commit -m "[commit-message]"
git commit -a -m "[commit-message]"  # Stage and commit all changes
```


## Push Changes
```bash
git push origin [branch-name]                       # Push changes to a specific branch
git push origin HEAD                                # Push changes to the current branch
git push origin --all                               # Push all branches
git push origin --tags                              # Push all tags
git push origin :[branch-name]                      # Delete a remote branch
git push origin --delete [branch-name]              # Delete a remote branch
git push origin [source-branch]:[target-branch]     # Push changes from one branch to another
git push -u origin [branch-name]                    # Push and set upstream branch
```

## Pull Changes
```bash
git pull origin [branch-name]                       # Pull changes from a specific branch
git pull origin --all                               # Pull changes from all branches
git pull origin --tags                              # Pull changes from all tags
git pull origin [source-branch]:[target-branch]     # Pull changes from one branch to another
```
```bash
git fetch origin [branch-name]                      # Fetch changes from a specific branch
git fetch origin --all                              # Fetch changes from all branches
```

## Commit Message Types
- **feat**: A new feature introduction
- **fix**: A bug fix
- **chore**: Changes not related to fix/feature (e.g., updating dependencies)
- **refactor**: Code changes that neither fix bugs nor add features
- **docs**: Documentation updates (README, markdown files)
- **style**: Code formatting changes (whitespace, semi-colons)
- **test**: Adding or correcting tests
- **perf**: Performance improvements
- **ci**: Continuous integration changes
- **build**: Build system or external dependency changes
- **revert**: Reverting a previous commit

```bash
Example: FIX*[foo]: fix foo to enable bar
This fixes the broken behavior of the component by doing xyz.
BREAKING CHANGE
Before this fix foo wasn't enabled at all, behavior changes from <old> to <new>

Closes D2IQ-12345
```

## View Commit History
```bash
git log
```

```bash
git log --oneline                               # Shortened commit history
git log --oneline --graph                       # Graphical representation of commit history
```

```bash
git log --amend                                 # Modify the last commit
git log --amend -m "New commit message"         # Change the commit message
git log --amend --no-edit                       # Modify the last commit without changing the message
```

## Git Branching

```bash
git branch                                      # List all branches
git branch [branch-name]                        # Create a new branch
git branch -M [branch-name]                     # Rename the current branch
git branch -a                                   # List all branches (local and remote)
git branch -r                                   # List remote branches
git branch -vv                                  # List branches with last commit info
git branch --merged                             # List merged branches
git branch --no-merged                          # List unmerged branches
```

```bash
git switch [branch-name]                        # Switch to a branch
git switch -c [branch-name]                     # Create and switch to a new branch
```

```bash
git checkout [branch-name]                      # Switch to a branch (deprecated)
git checkout -b [branch-name]                   # Create and switch to a new branch
```

```bash
git branch -d [branch-name]                     # Delete a branch
git branch -D [branch-name]                     # Force delete a branch
```

```bash
git branch -m [new-branch-name]                     # Rename a branch
git branch -m [old-branch-name] [new-branch-name]   # Rename a branch
```

## Git Merging
```bash
git merge [branch-name]                        # Merge a branch into the current branch
git merge --no-ff [branch-name]                # Merge with a no-fast-forward option
```

## Git Diff
```bash
git diff                                            # Show changes in the working directory
git diff HEAD                                       # Show changes between working directory and last commit
git diff --staged                                   # Show changes between staged and last commit
git diff --cache                                    # Show changes between last commit and working directory
git diff [file-name] [Head | --staged | --cache]    # Show changes in a specific file
git diff [branch 1] [branch 2]                      # Show differences between two branches
git diff [commit 1] [commit 2]                      # Show differences between two commits
```

## Git Stashing
```bash
git stash                       # Stash changes
git stash list                  # List stashes
git stash pop                   # Apply the latest stash and remove it
git stash pop [stash@{n}]       # Apply a specific stash and remove it
git stash apply                 # Apply the latest stash
git stash apply [stash@{n}]     # Apply a specific stash
git stash drop [stash@{n}]      # Remove a specific stash
git stash clear                 # Remove all stashes
git stash save "message"        # Stash with a message
git stash show                  # Show the latest stash
git stash show -p               # Show the latest stash with patch
git stash show [stash@{n}]      # Show a specific stash

```

## Undoing Changes
```bash
git checkout [commit-hash]                  # Checkout a specific commit
git checkout [Head~n]                       # Checkout the nth previous commit
git checkout [filename]                     # Checkout a specific file from the last commit
git checkout Head                           # Checkout the last commit
git checkout Head [filename]                # Checkout a specific file from the last commit
git checkout --staged [filename]            # Unstage a file
```

```bash
git switch -                                # Switch to the previous branch
```

```bash
git restore [file-name]                                         # Restore a file to the last commit
git restore --source [commit-hash | Head~n] [file-name]         # Restore a file from a specific commit
git restore --staged [file-name]                                # Unstage a file
```

```bash
git reset [commit-hash]                                 # Reset to a specific commit
git reset --hard [commit-hash]                          # Hard reset to a specific commit
```

```bash
git revert [commit-hash]                                # Revert a specific commit
```

## Git Rebase
```bash
git rebase [branch-name]                                # Rebase the current branch onto another branch
git rebase -i [branch-name]                             # Interactive rebase
git rebase --continue                                   # Continue after resolving conflicts
git rebase --abort                                      # Abort the rebase
git rebase --skip                                       # Skip the current commit
```
## Edit Commit History
```bash
git rebase -i HEAD~n                                 # Interactive rebase for the last n commits
```

## Interactive Rebase Commands
Commands used during interactive rebase (`git rebase -i`):

```bash
# Commands:
reword   # Change the commit message
edit     # Stop for amending the commit
pick     # Keep the commit as is
squash   # Meld into previous commit and edit message
fixup    # Meld into previous commit, discard message
exec     # Run a command
drop     # Remove the commit
```

Example usage:
```bash
pick abc1234 First commit
reword def5678 Second commit
squash ghi9012 Third commit to combine
fixup jkl3456 Fourth commit to combine silently
drop mno7890 Fifth commit to remove
```

## Git Tags
```bash
git tag -l                                      # List all tags
git tag -l "[pattern]"                          # List tags matching a pattern eg: *beta*, 17*, *12
git tag [tag-name]                              # Create a new tag
git tag -d [tag-name]                           # Delete a tag
git tag -a [tag-name] -m "[message]"            # Create a new tag with a message
git show [tag-name]                             # Show tag details
git tag [tag-name] [commit-hash]                # Tag a specific commit
git tag -f [tag-name]                           # Force tag the latest commit
git tag -f [tag-name] [commit-hash]             # Force tag a specific commit
git push origin [tag-name]                      # Push a specific tag
git push origin --tags                          # Push all tags
git push origin --delete [tag-name]             # Delete a remote tag
```







## Configure User Information
```bash
git config --global user.name "[name]"         # Set global username
git config --global user.email "[email]"       # Set global email
git config --global core.editor "[editor]"     # Set global code editor
```
```bash
git config --global user.name "[name]"         # Set local username
git config --global user.email "[email]"       # Set local email
git config --global core.editor "[editor]"     # Set local code editor
```

## Configure Git Settings Local
```bash
git config --local user.name "[name]"         # Set local username
git config --local user.email "[email]"       # Set local email
git config --local core.editor "[editor]"     # Set local code editor
```
```bash
git config --local color.ui true                    # Enable color in git commands
git config --local color.branch.current yellow      # Set color for current branch
git config --local color.branch.local yellow        # Set color for local branches
git config --local color.branch.remote yellow       # Set color for remote branches
git config --local color.status.changed yellow      # Set color for changed files
git config --local color.status.unmerged yellow     # Set color for unmerged files
git config --local color.status.untracked yellow    # Set color for untracked files
git config --local color.status.added yellow        # Set color for added files
git config --local color.status.deleted yellow      # Set color for deleted files
git config --local color.status.renamed yellow      # Set color for renamed files
git config --local color.status.type yellow         # Set color for type of files
git config --local color.status.header yellow       # Set color for header
```

## Git Hashing functions
```bash
git hash-object -w [file-name]                      # Hash a file
git hash-object -w -t [type] [file-name]            # Hash a file with a specific type
git hash-object -w -t [type] -s [size] [file-name]  # Hash a file with a specific type and size
```
```bash
git cat-file [hash]                        # Show the content of a file using its hash
git cat-file -p [hash]                     # Pretty print the content of a file using its hash
git cat-file -t [hash]                     # Show the type of a file using its hash
git cat-file -s [hash]                     # Show the size of a file using its hash
git cat-file -e [hash]                     # Check if a file exists using its hash
git cat-file -e [hash] [file-name]         # Check if a file exists using its hash and name
```
```bash
git cat-file -p [hash] > [file-name]                # Save the content of a file using its hash to a file
git cat-file -p [hash] > [file-name] 2> /dev/null   # Save the content of a file using its hash to a file and suppress errors
```

```bash
git cat-file -p master^{tree}                               # Show the content of the master branch
git cat-file -p master^{tree} > [file-name]                 # Save the content of the master branch to a file
git cat-file -p master^{tree} > [file-name] 2> /dev/null    # Save the content of the master branch to a file and suppress errors
```
## git Commit
```
Commit: {
    tree: [tree-hash],              # Hash of the tree object
    parent: [parent-hash],          # Hash of the parent commit
    author: [author-name],          # Author of the commit
    committer: [committer-name],    # Committer of the commit
    message: [commit-message],      # Commit message
}

```
```bash
git cat-file -p [commit-hash]                               # Show the content of a commit using its hash
git cat-file -p [commit-hash] > [file-name]                 # Save the content of a commit using its hash to a file
git cat-file -p [commit-hash] > [file-name] 2> /dev/null    # Save the content of a commit using its hash to a file and suppress errors
```

## Git Reflog
```bash
git reflog                                      # Show the reflog
git reflog show                                 # Show the reflog for the current branch
git reflog show Head@{n}                        # Show the reflog for a specific commit
git checkout Head@{n}                           # Checkout a specific commit from the reflog
git diff Head@{n}                               # Show the differences between the current commit and a specific commit from the reflog
git refog show Head@{yesterday}                 # Show the reflog for yesterday options: yesterday, today, tomorrow 2.day.ago 
git reset Head@{n}                              # Reset to a specific commit from the reflog
git reset --hard Head@{n}                       # Hard reset to a specific commit from the reflog
git reset --soft Head@{n}                       # Soft reset to a specific commit from the reflog
```

## Git Aliases
```bash
git config --global alias.[alias-name] "[command]"  # Create a global alias
git config --local alias.[alias-name] "[command]"   # Create a local alias
git config --local alias.st status                  # Create an alias for status
git config --local alias.co checkout                # Create an alias for checkout
git config --local alias.br branch                  # Create an alias for branch
```

# Ignore files in the repository
## Git Ignore Files
```bash
.gitignore                           # File to specify which files Git should ignore
```

### Common .gitignore patterns
```bash
# Ignore specific files
filename.txt
path/to/file.txt

# Ignore files by extension
*.log
*.tmp
*.swp

# Ignore directories
node_modules/
dist/
build/
.cache/

# Ignore all files in a directory
directory/*

# Keep specific files in ignored directory
!directory/.gitkeep

# Ignore files by pattern
**/debug.log
temp_*
```

### Managing .gitignore
```bash
git check-ignore [file-name]            # Test if a file is ignored
git add -f [file-name]                  # Force add ignored file
git rm --cached [file-name]             # Untrack file without deleting
```

### Global Git Ignore
```bash
git config --global core.excludesfile ~/.gitignore_global   # Set global gitignore
```
## Common .gitignore templates

### React
```bash
# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

### Python
```bash
# Byte-compiled
__pycache__/
*.py[cod]
*$py.class

# Distribution / packaging
dist/
build/
*.egg-info/

# Virtual environments
venv/
env/
.env/
.venv/

# IDE
.idea/
.vscode/
*.swp
*.swo

# Unit test / coverage
htmlcov/
.tox/
.coverage
.coverage.*
```

### JavaScript
```bash
# Dependencies
node_modules/
npm-debug.log
yarn-debug.log
yarn-error.log

# Build
dist/
build/

# Environment
.env
.env.local
.env.*.local

# IDE
.idea/
.vscode/
*.suo
*.ntvs*
*.njsproj
*.sln
```
