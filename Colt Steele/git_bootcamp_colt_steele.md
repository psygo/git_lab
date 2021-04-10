# Git Bootcamp by Colt Steele

> [Link to the Course][colt_udemy]


[colt_udemy]: https://www.udemy.com/course/git-and-github-bootcamp

## Some Interesting Facts

1. Git is *the* VCS.
1. It ranks at 88.4%, the second is Subversion with 16.6% (2018).

## History

Possible explanations for the name:

- random 3-letter combination that is pronounceable, and not actually used by any common UNIX command.
- stupid, contemptible, despicable, simple.
    - It means "unpleasant person" in British English. Linus: "I'm an egotistical bastard, and I name all my projects after myself".
- "global information tracker"
- "goddamn idiotic truckload of shit" when it breaks

## Who Uses Git?

You can use [Stackshare][stackshare] to check out other people who use Git.

There are even government departments using them for writing laws.

And many writers and symphony composers have been using it.


[stackshare]: https://stackshare.io/

## Git GUI Clients

You can track them [here][git_gui].


[git_gui]: https://git-scm.com/download/gui/linux

## Committing

Working Directory -> `git add` -> Staging Area -> `git commit` -> Repository

## Commit Messages

The git docs actually recommends *imperative mood* instead of past tense.

Git's CLI message actually are done with imperative tense.

## Ignoring Files

On Macs, the `.DS_Store` files are OS-level files created automatically.

## HEAD

Probably comes from the play heads on tape, physical recorders.

## Working with Branches

There's a newer command for switching branches: `git switch <branch-name>`. `git checkout` had way too many functionalities.

- `git switch -c <branch-name>` creates a branch and switches to it. This helps in avoiding the mistake of creating a branch but forgetting to switch.
    - Or `git checkout -b <branch-name>`.
- If, when switching branches, there are conflicts with stuff in the working directory, Git might yell at you anyways.
- `git branch -m <branch> <new-name>` renames a branch.

> [There is a way of recovering deleted branches.][recover_deleted_branches]


[recover_deleted_branches]: https://stackoverflow.com/q/3640764/4756173

### Studying the Internals of Branches

If you go to `.git` and then `cat HEAD`, it will simply list the current address/branch, e.g.: `ref: refs/head/master`. And `refs/head/master` will simply point to the hash of the top-most commit.

## Mergin Branches

- We merge branches, not specific commits.
- We always merge to the current HEAD branch.

## Git Diff

Git diff summary:

1. `diff --git A B`
    - A isn't necessarily the older file, it is chosen with respect to the branches.
1. hashes of both files
1. File A will be represented typically with `---` and file B with `+++`.
    - It will show `--- /dev/null` if the file didn't exist.
1. Chunks for each file.
    - The header shows how many lines have been extracted/inserted into each file (A and B).
    - `@@ -3,4 +3,5 @@` -> 4 lines have been extracted from file A, starting at line 3 (...).

- Run `git diff HEAD` to list all the differences between the working directory and the HEAD.

- `git diff --staged` (or `--cached`) will list the changes between the staging area and our last commit.

- `git diff branch1..branch2` = `git diff branch1 branch2` compares 2 branches &mdash; the order does matter.
    - It's similar for commits.

## Git Stash

- `git stash` = `git stash save`
- With `git stash apply`, the changes still stay in the stash, you can apply them multiple times.
    - Conflicts work the same way they do when resolving conflicts from merges.
- `git stash list` shows what's in the stash's stack.
- `gist stash apply stash@{id}` applies a specific stash on the stack.

## Undoing Changes & Time Traveling

- Detached HEAD: you can commit and do stuff without affecting any branches.
    - Usually, HEAD points to a branch, not a commit.
    - **Reattaching** the HEAD:
        1. You can use `git switch` to go back to a regular branch.
        1. `git switch -c <new-branch>` at the detached HEAD to create a new branch there.
- `git restore`
- You can reference commits with respect to HEAD: `HEAD~1` is the commit right before HEAD for example.
- `git switch -` goes back to the previous branch, it's useful for when you're in a detached HEAD state.
- `git checkout HEAD <file>` = `git checkout -- <file>` erases the current work and overwrites it with the last HEAD commit.
    - `git restore` is a newer version of this workflow.
        - `git restore <file>`
        - `git restore --source HEAD~1 app.js` moves back to the previous commit on only the file `app.js`.
- `git restore` can also be used to unstage stuff.
    - `git restore --staged <file-name>`
- `git reset <commit-hash>` will reset the repo to that commit.
    - The changes will persist, what will not is the existence of the commits.
    - This is useful if you commit the wrong stuff to a branch.
    - `--hard` also gets rid of the changes, they will not appear in the working directory.
    - This is a per branch procedure.
- `git revert` actually creates a new commit based on a previous one.
    - It will prompt the user for a commit message even.
    - Using `git reset` in teams might be difficult to reconcile.
        - If you only have the work locally, it might be easier to use `git reset`.

## Pushing and Pulling

- `git push <remote> <local-branch>:<remote-branch>`
    - `git push remote origin master` = `git push remote origin master:master`
- `git push -u origin master` ties the upstream of the local master to the remote master, so you can directly use `git push` as a shortcut.
    - This works on a per branch basis.
    - Example: `git push -u origin dogs:cats`
- `git branch -M main` renames the current branch (probably `master`) to `main`, which is the now recommended name by Github.

### Fetching and Pulling

- Remote branches: `<remote>/<branch>`.
    - `git checkout origin/main` will track the *last time* you communicated with the remote branch.
- `git branch -r` shows remote branches.
- By default, only the default branch (`master`/`main`) will be connected to the remote when you clone.
    - However, if you use `git switch`, `git` will create the associations on the fly.
    - The older way of doing this was `git checkout --track origin/<branch-name>`.
- `git fetch` takes remote changes and brings them into the local repository.
    - They will be stored in the remote branches.
    - `git status` might say we are up to date, but we are only up to date with what we currently know of the remote.
    - `git fetch origin` updates all remote branches at once.
        - This will also show you if there are any new branches on the remote.
- `git pull` takes remote changes and brings them into the repository all the way into the workspace as well.
    - `git fetch` + `git merge`
    - You can get away simply with `git pull` if you're using the `origin` usual default and if there's a tracking reference.

### Github and Remotes

> Pull Requests are not a native part of Git itself.

- Github does offer a way of resolving conflicts on the browser.
    - But you can also fetch contents from the new branch Github creates when conflicts appear as a result of troublesome PRs.
    - Follow the in-browser instructions.
- Branch protection even features patterns, since there might be thousands of branches in a big organization.

## Git Rebase

- It's an alternative to merging.
- A good tool for cleaning up.

If the master branch is very active, you might need to merge things every time to keep your code up-to-date. This will make your feature branch very noisy in terms of commit messages.

Rebasing then rewrites the history to linearize things more cleanly:

```sh
git switch feature
git rebase master
```

The code above will rebase based on the *tip* of the master branch.

If there are conflicts, resolve them and then `git rebase --continue`.

### When NOT to Rebase

**Do not rebase unless you are positive no one on the team is using those commits**.

### Cleaning Up Manipulating Git History with Interactive Rebase

> Still do apply the golden rule for rebasing.

Use the `-i` flag for interactivity.

`git rebase -i HEAD~4` will rebase interactively up to 4 previous commits. (oldest to newest)

> All of the commands will appear as helpers in the rebasing window.

- `pick`   -> use commit
- `reword` -> use commit but edit the commit message
- `edit`   -> use commit but stop for amending (similar to `fixup`)
- `squash` -> use commit, but meld into previous commit
- `drop`   -> remove commit, including its contents

If you did something wrong, you can undo it afterwards by looking at the command logs in `git reflog`.

## Tags

### Semantic Versioning

[The original document][semver]

`<major>.<minor>.<patch>
`
- Version `1.0.0` is first definition of the public API.
- Minor features new functionality, which won't break the major release.
- Do reset patch releases when releasing minors.
- Major releases might break current software.


[semver]: https://semver.org/

### Working with Tags

- `git tag` lists all the tags.
- `git tag -l "*beta*"` looks for tags with beta in them.
- `git diff v17.0.0 v17.0.1`
- `git tag <tagname>`
    - The annotated version: `git tag -a <tagname>` will open an editor for the tag's annotation.
- `git show v17.0.0`
- You can move tags with the `-f` flag.
- `git tag -d delete-me`
- `git push --tags` pushes all of the tags.
    - `git push origin my-tag` to push a single tag.

## Git Behind the Scenes


