+++
title = "Git"
date = 2024-05-01

[taxonomies]
tags=["devops", "programming"]
+++

## Git

- **[How to modify someone else's Github pull request?](https://stackoverflow.com/a/74631110)**
- **[Remove tracking branches no longer on remote](https://stackoverflow.com/questions/7726949/remove-tracking-branches-no-longer-on-remote)**
- [Push New local branch to remote(origin)](https://stackoverflow.com/questions/1519006/how-do-i-create-a-remote-git-branch/27185855#27185855)
- [Warning/Tips/Caution/Notes in Github’s README](https://github.com/orgs/community/discussions/16925)
- git submodules
- [Semantic Commit Messages](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716#file-semantic-commit-messages-md)
- Multiple Remotes
- Add SSH key:
  - [SSH-key on MacOs](https://apple.stackexchange.com/questions/48502/how-can-i-permanently-add-my-ssh-private-key-to-keychain-so-it-is-automatically)
  - [Generate SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
  - [Add SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- Usual workflow
  - For every new feature a new branch is created, the feature is developed in the new branch, then a Pull Request is performed to integrate the feature branch with the main code.
  - The Maintainer, reviews the feature branch and performs a “Merge” to integrate the changes. Before the Merge, some comments and modifications may occur. Conflicts may arise.
  - Sometimes, 2 “main” branches are used. The proper main branch and the develop branch, an extra branch may be used to solve hot-fixes.
- HEAD always points to the most recent commit which is reflected in the working tree.
- Git has relative refs:
  - Moving upward one commit at a time: `^`
  - Moving upwards a number of times with `~<num>`
- `git reverse HEAD` (remote)
- `git reset <commit>` (local)
- `git cherry-pick <commitA> <commitB>` it “copies” some commits, even if they are in other branches, below HEAD.
- `git rebase -i <commit>` interactive way of reordering commits.
- Git Remote:
  - importance of origin/main. `origin` is the default **alias** to the URL of your remote repository.
  - `git push origin <branch>` // `git push origin <source>:<destination>` (this command creates the destination branch if it doesn’t exist)
  - `git clone` `git fetch`
  - `git pull` == `git fetch; git merge o/main` // `git pull origin <source>:<destination>`
  - `git pull --rebase; git push` (useful in some cases)
  - If new feature is implemented, but the remote branch has some changes:
    - create a feature branch: `git checkout -b feature`
    - checkout and reset the branch that has changed: `git checkout main; git reset HEAD^` (instead of HEAD^ an specific commit can be used if multiple changes has been made)
    - finally: `git checkout feature; git push`
- Git commit message: it concisely describes the why of some decisions and what has been changed.
  - [SetUp a text editor](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) to write a good commit message that consist of a Subject and Body.
  - `git log` → displays commit history
    - `git shortlog`
    - `git log --oneline` → Shows only the Subject.
  - [Separate subject from body with a blank line](https://cbea.ms/git-commit/#separate)
  - [Limit the subject line to 50 characters](https://cbea.ms/git-commit/#limit-50)
  - [Capitalize the subject line](https://cbea.ms/git-commit/#capitalize)
  - [Do not end the subject line with a period](https://cbea.ms/git-commit/#end)
  - [Use the imperative mood in the subject line](https://cbea.ms/git-commit/#imperative)
  - [Wrap the body at 72 characters](https://cbea.ms/git-commit/#wrap-72)
  - [Use the body to explain *what* and *why* vs. *how*](https://cbea.ms/git-commit/#why-not-how)
- Rebase vs Merge:
  - Rebase is useful to have a cleaner history free of unnecessary merge commits.
  - The golden rule of `git rebase` is to never use it on *public* branches.
  - `git push --force` only if no one is using the branch.
  - If you use pull requests as part of your code review process, you need to avoid using `git rebase` after creating the pull request. For this reason, it’s usually a good idea to clean up your code with an interactive rebase *before* submitting your pull request.
- [Configure Line Endings](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings)

`git config --list` command to list all the settings Git can find at that point

`git config <key>`

`git config --show-origin <key> (shows the file being used, global or local)`

`git help <command>`

- Blank lines or lines starting with # are ignored.
- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
- You can start patterns with a forward slash (/) to avoid recursivity.
- You can end patterns with a forward slash (/) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (!)

`git commit -a` (it isn't necessary a previous git add, the -a option stages every file that is already tracked)

`git rm <file>` removes file locally and stages the file's removal.

if the files have been staged, the `--cached` flag "undoes the staging".

`git mv <file1> <file2>` == `mv <file1> <file2>`; `git rm <file1>`; `git add <file2>`

Useful if performing a code review:

`git log --pretty=format:<format>`

`git log -p` (shows the lines changed)

`git log --oneline --graph`

`git log --oneline --decorate --graph --all`

`git log --since=1.weeks`

if you wanted to find the last commit that added or removed a reference to a specific function, you could call: `git log -S <function>`

check if a file was modified: `git log -- <path/file>`

`git commit --amend` use case:

```bash
git commit -m 'msg'
git add forgotten_file
git commit --amend
```

The same commit msg is used, but the previous commit is replaced by the commit after the add command.

> [!WARNING]
> Only amend commits that are still local and have not been pushed somewhere.

It’s important to understand that `git checkout -- <file>` or `git restore <file>` is a dangerous command. Any local changes made to that file are gone.

`git clone` command implicitly adds the origin remote for you. origin — is the default name Git gives to the server you cloned from.

Tags have to be pushed: `git push origin <tagname>` `git push origin --tags` to push all tags

Git Aliases: `git config --global alias.<alias> '<command>'`

Example: `git config --global alias.last 'log -1 HEAD'`

Rebase:
Often, you’ll do this to make sure your commits apply cleanly on a remote branch — perhaps in a project to which you’re trying to contribute but that you don’t maintain. In this case, you’d do your work in a branch and then rebase your work onto origin/master when you were ready to submit your patches to the main project. That way, the maintainer doesn’t have to do any integration work — just a fast-forward or a clean apply.

Page 99 - ProGit → use case.

“**Do not rebase commits that exist outside your repository and that people may have based
work on.”**

`git log -- <file> --oneline | wc -l` → Determine the number of commits that have changed the file.

`git diff <commit1> <commit2> -- <file>` → diff of specific file between commit1 and commit2.

`git diff --stat <commit1> <commit2> -- <file>` → number of changes.

[Useful exercises](https://jvns.ca/blog/2019/08/30/git-exercises--navigate-a-repository/)
