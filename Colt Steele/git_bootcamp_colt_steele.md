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
