# Basic git

1. Make sure your local copy of the selected branch is updated.
   - `git pull`
2. Check your repo branches
   1. Local branches
      - `git branch`
   1. All branches on remote repo
      - `git branch -r`
3. Create a branch and access it
   1. Normal way
      1. `git branch new_branch`
      2. (2 ways)
         -  `git checkout new_branch`
         -  `git switch new_branch`
   2. Shortcut (2 ways)
      - `git checkout -b new_branch`
      - `git switch -c new_branch`
4. Get some work done lol
5. Check the status of your work
   - `git status`
6. Add changes to staging in order to prepare your commit
   1. Add a single file
      - `git add new_file.txt`
   2. Add all changed files
      - `git add . -p`
7. Did you screw up? Reset the staging
   - `git reset`
8. Commit
   - `git commit -m "This is a commit message"`
9. Check the commit history of the branch you're in
   - `git log`
10. Make sure you upload your commits to the remote repo! If your local branch is brand new, you must add it to the remote repo.
    1. New branch
       - `git push -u origin new_branch`
    2. Previously existing branch
       - `git push`
11. Move to another branch
    - `git checkout another_branch`
12. Merge some branch into your current branch (assuming default behavior of pull is merge)
    - `git pull branch_that_will_be_merged_into_current_branch`

For more info check the [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)

# Advanced git

The following are some best practices that may be useful, taken from [this blog post](https://mislav.net/2013/02/merge-vs-rebase/)

1. While working on a branch, if you need to pull commits from the remote repo to your local repo, use rebase instead of merge to reduce the amount of commits
   - `git pull --rebase`
   - If you want to make rebasing the default behavior when doing `git pull`, do so with `git config --global --bool pull.rebase true`
2. Before pushing your changes to the remote repo, perform basic houseleeping (squash related commits together, rewording messages, etc)
   - `git rebase -i @{u}`
3. Merge (do not rebase) changes from master/main into your branch, in order to update the branch with the latest features and solve any compatibility issues and/or conflicts
   1. `git merge main`
   2. `git pull --merge main`
4. Enforce merge commit when merging feature branch into main, even if a merge commit isn't necessary (check next point for exception), in order to make it easier to see the where and when of changes. Assuming you're in main:
   - `git merge --no-ff branch_that_will_be_merged_into_main`
5. Exception to point 4: if you only need to merge a single commit (typical for stuff such as bugfixes). Assuming you're in main:
   - `git cherry-pick branch_that_only_has_a_single_commit`
6. Delete merged branch:
   1. Delete locally 
      - `git branch -d branch_that_has_been_merged`
   1. Delete on remote repo
      - `git push origin :branch_that_has_been_merged`

# Create a remote repo (local folder as remote repo)

## Official method

_[Source](https://git-scm.com/book/en/v2/Git-on-the-Server-Getting-Git-on-a-Server)_

1. Make sure you've got a local commit. You may initialize a local repo with `git init` on any project folder and making sure that it has at least one commit, or you may use an already existing local repo.
2. On a separate folder, run:
   ```bash
   git clone --bare path/to/local/project project.git
   ```
   * This will create a folder with name `project.git` on the folder you're running the command.
   * Remote repo folders use the `.git` extension as a standard.
   * This folder is a ***bare*** repository. It does not contain a working folder, only the git files.
3. Move the `project.git` folder to the final destination. Ideally, a shared folder such as a networked drive that everyone has access to "locally".
   * You may combine steps 2 and 3 by creating the bare repo directly on the final folder.
4. You should now be able to clone the repo:
   ```bash
   git clone path/to/remote/repo/project.git
   ```
5. The original repo that we bare-cloned does not have an origin repo to push to. If you want to keep using it, set up a remote like this:
   ```bash
   git remote add origin path/to/remote/repo/project.git
   ```

## Alternative method

_[Source](https://stackoverflow.com/questions/14087667/create-a-remote-git-repo-from-local-folder)_

1. On remote folder:
   ```bash
   mkdir my_repo
   cd my_repo
   git init --bare
   ```
2. On local folder:
   ```bash
   cd my_repo
   git init
   git remote add origin ssh://myserver/my_repo
   git add .
   git commit -m "Initial commit"
   git push -u origin master
   ```