# Learning Git

This guide will help you through understanding Git. I'm having difficulty wrapping my head around it, and I'm hoping this will help solidify my memory slightly. The best way to proceed is likely to cover what each command does. I will try to do so in a visual way. For a more interactive way of learning Git, I recommend checking out the following website: [https://learngitbranching.js.org](https://learngitbranching.js.org)

## Commands

- [Basics](#basics)
  - [git commit](#commit)
    - [--amend](#amend)
  - [git branch](#branch)
    - [Moving Branches](#moving-branches)
  - [git checkout](#checkout)
    - [References](#references)
      - [HEAD](#head)
      - [Caret](#caret)
      - [Tilde](#tilde)
    - [Chaining](#chaining)
  - [git merge](#merge)
  - [git rebase](#rebase)
    - [Interactive Rebase](#interactive-rebase)
- [Controlling branches](#controlling-branches)
  - [git reset](#reset)
  - [git revert](#revert)
  - [git cherry-pick](#cherry-pick)
  - [git tags](#tags)
    - [git describe](#describe)
- [Remotes](#remotes)
  - [git clone](#clone)
    - [Remote branches](#remote-branch)
    - [Remote tracking](#remote-tracking)
  - [git fetch](#fetch)
  - [git pull](#pull)
  - [git push](#push)
    - [Divergent History](#divergent-history)
  - [Protected Branches](#protected-branches)

## Basics

First off, we should try to visualize Git as a working tree for your files and folders. It is a way to keep track of changes to your files in development.

### Commit

So let's say you want to take a "snapshot" of your files and folders in their current form. You're going to use a command called `git commit`.

`git commit` takes a "snapshot" of your selected files and folders, and "saves" them in Git.

When you initialize Git, you start with a `main` branch; an empty work tree. When you commit in a branch (such as `main`), you create a new "snapshot" for that branch, which can then be referenced later.

```
            C0 - git init (empty)
            |
            C1 - *main (after committing once)
```

#### Amend

If you type the option `git commit --amend`, you can correct the most recent commit with the changes you need.

### Branch

It's generally bad practice to ALWAYS work from your `main` branch. Let's imagine for a moment that you're working on a website or a piece of software, and `main` is the "live" product. Do you want to "test" new features in the "live" version of your work? What would happen if your test breaks the "live" version? You can see how this can get messy.

For that reason, we can create new branches to our worktree. A branch creates a "copy" of our current commit in which you can play without affecting the other branches. You can achieve this by typing `git branch <branch name> <location>`. If you want to create a development branch, you could create a branch called `dev` with `git branch dev`. You can also specify where to create the branch if you have multiple commits with the `<location>` (such as `git branch dev HEAD~^2~`) argument. I recommend reading [chaining](#chaining) to learn more about navigating around the worktree.

```
            C0
            |
            C1 - *main & dev
```

`main` & `dev` are now two branches, but they refer to the same commit. Next time you commit, they will both branch separately.

#### Moving branches

Once you understand [references](#references), they can be used to move branches around the worktree to previous commits. You can do so with the command `git branch -f <branch name> <commit>`. The `<commit>` can either be to another branch name, a hash, or use the caret or tilde commands. The `-f` just means "force".

```
Example #1 - Moving main up three commits    Example #2 - Moving to branch

            C0                                  C0
            |                                   |
            C1 - main                           C1
            |                                  /  \
            C2                               C2    C3
            |                                |     |
            C3                       main' - C4    C5
            |                                      |
            C4 - *dev                              C6 - main & dev

    git branch -f main HEAD~3               git branch -f main dev
```

In the first example, if we assume `main` (and HEAD) was on the same commit as `dev`, main will move up three parents from HEAD. In the second, we moved main, which was on a different branch, to our dev branch.

### Checkout

When you initialize a new Git repository (repo), you typically work within the `main` branch. By creating a new branch (such as `dev`), you can now jump between the two and separate your "live" product from your "development" work.

Let's say that you created a new branch, and now want to start working IN that new branch. To do so, you'll use the command `git checkout <branch name> <location>`. For example, if your new branch is called `dev`, then you'll write `git checkout dev`. You can also specify where you want to checkout a branch (such as a branch or commit). See [References](#references) for details.

```
Example #1 - Checkout new branch          Example #2 - Commit from branch

            C0                                          C0
            |                                           |
            C1 - main & *dev                            C1 - main
                                                        |
                                                        C2 - *dev
```

The `*` denotes which branch you are currently working in.

_You can actually create a new branch AND checkout that branch at the same time by using the command `git checkout -b <branch name> <location>`._

If you `git commit` while checked out in `dev`, you'll end up with a working tree like in Example #2.

Notice how the `main` branch is now behind `dev`. That's because `main` still refers to its respective commits even if you're working in other branches. Your "live" product (in `main`) is safe from any work you're doing in `dev`.

#### References

Each commit in a git repo has a unique identifier, called a `hash`. It's usually a long string of letters and numbers like (such as `472dadae3b2575fb313e12854efab28e2ebe92b3`). As they're all unique however, you can refer to them fairly quickly by relatively identifying them by their first digits. For example, the relative reference for the previously mentioned hash would be `472dada`.

##### HEAD

HEAD is just the way of referencing the currently checked out commit. Usually, HEAD will reside within the branch name, but we can change it to any commit we want. Let's say our current working tree looks like this, and we're currently checked out in `dev`.

```
Example #1 - HEAD > dev > C4                    Example #2 - HEAD > C4

            C0                                              C0
            |                                               |
            C1                                              C1
           /  \                                            /  \
  main - C2    C3                                 main - C2    C3
               |                                               |
               C4 - *dev                                HEAD - C4 - *dev

                                                    git checkout C4
```

If we want HEAD to refer to a particular commit, we'll want to refer it to the appropriate hash. We'd do so with the `git checkout <relative hash>` command.

##### Caret

As previously mentioned, HEAD usually resides in the same commit as the branch. In this example:

```
            C0
            |
            C1
           /  \
  main - C2    C3
               |
               C4 - *dev
```

HEAD > dev > C4

With the caret `^`, we can move HEAD up the worktree to previous commits. The `^` will refer to the current commit's parent.

```
Example #1 - One Caret                  Example #2 - Multiple Carets

            C0                                      C0
            |                                       |
            C1                                      C1 - HEAD
           /  \                                    /  \
  main - C2    C3 - HEAD                  main - C2    C3
               |                                       |
               C4 - *dev                               C4 - *dev

    git checkout dev^                       git checkout dev^^
```

The more `^` we add, the higher up it goes. This could also be achieved by typing `git checkout HEAD^` twice.

Optionally, you can add a number to the caret `^<#>`, such as in `git checkout dev^2`. In the examples above, it wouldn't accomplish much. In the case where a commit references to another branch (in the case of a [merge](#merge) for example), then HEAD would follow the branch route, as opposed to the `main` route. A `^3` would follow the third branch, and so on. See [chaining](#chaining) for a more concrete example.

##### Tilde

The Tilde `~` allows you to move up the worktree much quicker. Instead of typing multiple `^`, you can simply write a `~<#>` to specify how many commits to ascend.

```
Example #1 - Using the Tilde

            C0 - HEAD
            |
            C1
            |
            C2
            |
            C3
            |
            C4 - main

    git checkout HEAD~4
```

#### Chaining

You can actually chain the caret `^` and the tilde `~` in a single command to move HEAD along the worktree. For example:

```
Example - Chaining ^ & ~

            C0
           /  \
         C1    C3 - HEAD
         |     |
         C2    C4
         |     |
         |     C5
         |    /
         |   /
         |  /
         C6
         |
         C7 - *main

    git checkout HEAD~^2~2
```

Let's break down the command above by starting at `main`. We move up a single commit w/ the first `~`, then move to the 2nd branch w/ `^2` (C5), then move up another two commits w/ `~2`. HEAD ends up at C3.

### Merge

Let's say you've done some work in a branch (like `dev`), and are now ready to add them to your `main` branch (your "live" product). Wouldn't it be nice to _merge_ them together? The command `git merge <branch name>` allows us to do just that.

Merging can be a little complex to visualize and easy to "mess up". For the sake of simplicity, we'll only use two branches (`main` and `dev`).

```
            C0
            |
            C1
           /  \
  *dev - C2    C3 - main
```

We have two branches with separate commits. While you were working on `dev` (currently checked out), a colleague pushed some new work in the `main` branch. Now it's time to merge your work into `main`.

```
Example #1 - Merging from branch            Example #2 - Merging from main

            C0                                          C0
            |                                           |
            C1                                          C1
           /  \                                        /  \
         C2    C3                              dev - C2    C3
            \  |                                        \  |
             \ |                                         \ |
               C4 - main & *dev                            C4 - *main

    git merge main                                  git checkout main
                                                    git merge dev
```

Since you were working in `dev` and merged into `main`, Git simply brings your current branch to a new commit along with `main`. However, if you wanted to keep `dev` in it's current location, you can checkout in `main` first and then merge `dev`. **In most cases, you'd likely want the first scenario to stay as up-to-date as possible w/ main.**

### Rebase

It's easy to forget that Git is actually a tool for version control, and a convenient way to keep track of changes and file history in development. When merging branches, the history of your changes will to its respective branches. But what if you wanted to "pull" the whole history from a working branch (such as `dev`), and visually see them in a linear fashion in your "live" product (like `main`)? This is when `git rebase <branch name>` would come into play. For consistency and simplicity, let's start with our previous example.

```
            C0
            |
            C1
           /  \
  *dev - C2    C3 - main
```

```
Example #1 - Rebasing main             Example #2 - Checkout main & rebasing

            C0                                          C0
            |                                           |
            C1                                          C1
           /  \                                        /  \
         C2    C3 - main                             C2    C3
               |                                           |
               C2' - *dev                                  C2' - *main & dev

    git rebase main                              git rebase dev main
```

Rebasing will take the commit from your working branch, make a copy and paste it to your `main` branch. However, the `main` branch is now behind the development branch. To fix this, you can type the second branch first (like `dev`), followed by `main`.

In the second example, you can think of the first branch as the reference point, and the next branch you want to bring TO the reference point. In this case, you're bringing `main` -> `dev`.

#### Interactive Rebase

You can also rebase "interactively", which behaves much like `git cherry-pick` (see [git cherry-pick](#cherry-pick)), but it will open up a dialog to ask which commits to rebase. Interactive rebase allows you to do lots of things, such as: Combining commits, amending commit messages, edit commits, reorder commits, keep or drop commits altogether.

```
Example #1 - Init                Example #2 - Interactive Rebase

            C0                                  C0
            |                                   |
            C1                                  C1
            |                                  /  \
            C2                               C2    C4'
            |                                |     |
            C3                               C3    C3' - *main
            |                                |
            C4 - *main                       C4

                                       git rebase -i HEAD~3
```

In the example above, we're interactively rebased the `main` to _omit_ C2, and change the order of C4 & C3. You would perform this through an interactive dialog box.

## Controlling Branches

### Reset

`git reset` will move a branch back to a previous commit, and "erase" any commit up to that point. This is usually used at a **local** level.

```
Example #1 - Init                   Example #2 - Reset visualization

        C0                                      C0
        |                                       |
        C1                                      C1 - *main
        |
        C2 - *main

                                            git reset HEAD~1
```

The main reference point returns to the first commit as though the second never happened.

### Revert

Although `git reset` works well on local git repos, it doesn't really work when working through [remote branches](#remotes) where there are multiple team members (if you push your work on Github or Gitlab). If this is the case, `git revert` is a better option.

`git revert` doesn't roll back a commit like `reset`. Instead, it will push a _new_ commit which reverses the changes from the previous one.

```
Example #1 - Init                   Example #2 - Revert visualization

        C0                                      C0
        |                                       |
        C1                                      C1
        |                                       |
        C2 - *main                              C2
                                                |
                                                C2' - *main

                                            git revert HEAD
```

This way, anyone working on the project can see the `revert`, and it will be logged in the repos history.

### Cherry-pick

`git cherry-pick` performs quite similarly to `git rebase`. However, in the event where you want to selectively pick which commits to "rebase", then you want to use `git cherry-pick <commits...>`.

```
Example #1 - Init                Example #2 - Using cherry-pick

            C0                                  C0
            |                                   |
            C1                                  C1
           /  \                                /  \
         C2    C3 - *main                    C2    C3
         |                                   |     |
         C3                                  C3    C2'
         |                                   |     |
   dev - C4                            dev - C4    C4' - *main

                                       git cherry-pick C2 C4
```

In this example, we want to copy the C2 & C4 commits from `dev` into `main`. Making sure we're _checked out_ in `main` first, we `git cherry-pick C2 C4` and it will "rebase" those commits into `main`. In these examples, we only cherry-picked from two branches, but you can cherry-pick from as many branches as you'd like as you're specifying each commit individually.

### Tags

Tags are markers for Git repos. They permanently indicate a commit and can be referenced in a branch. It's important to keep in mind however that tags cannot be checked out, you can't work on them, and they don't move as commits are created. They are "anchors" in the worktree. You can do so with the command `git tag <tag name> <commit>`.

Tags are often used to indicate major releases or milestones in a git repo.

#### Describe

`git describe <ref>` will indicate where you are in relation to the closest tag. The `<ref>`, if left blank, will use HEAD as its reference point. Otherwise, you can refer to a commit, a branch or HEAD.

The output looks like:

    <tag>_<numCommits>_g<hash>

`<tag>` is the closest anchor in the history, `<numCommits>` is how many commits away that tag is, and `<hash>` is the associated hash for `<ref>`.

```
Example #1 - Describe main             Example #2 - Describe dev

            C0(v1)                                      C0(v1)
            |                                           |
            C1                                          C1
           /  \                                        /  \
  main - C2    C3(v2)                                C2    C3(v2)
               |                                           |
               C4 - *dev                                   C4 - *dev

    git describe main                              git describe dev
```

The output for _Example #1_ would be `v1_2_gC2`, while the output for _Example #2_ would be `v2_1_gC4`.

## Remotes

So far, most of the commands we've seen are used at a local level to control the worktree on your own computer. However, the power of Git repositories can be found online through platforms such as Github & Gitlab. Remotes are online "backups" and allow git repositories to become more interactive among the online community.

### Clone

`git clone` allows you to create a _local_ copy of a remote repository.

#### Remote branch

When you clone a repository, you end up with a new branch in your repository with a `<remote name>/<branch name>` convention. More often than not, this looks like `origin/main`. This is called a _Remote branch_. A remote branch will reflect the state of a remote repository (since you last referenced it). A remote branch is on your _local_ version of the repository.

When you checkout a remote branch, you actually operate in a detached HEAD mode since you CANNOT work on the branches directly. You work on them elsewhere (such as a local PC), then share it with the remote.

```
Example #1 - Checking out a Remote Branch

            C0
            |
            C1 - main & origin/main
            |
           (C2) - HEAD

    git checkout origin/main
    git commit
```

When we checkout origin/main and then commit, we are put in detached HEAD mode. `main` & `origin/main` are NOT affected by `git commit`; Only HEAD is. `origin/main` will only update once we update the remote repository.

#### Remote Tracking

When you clone a remote repository, `main` and `origin/main` become "connected". By this, I mean that when you do a `git pull` (see [git pull](#pull)), commits are downloaded on `origin/main` and then merged on `main`. On the flipside, when you `git push` (see [git push](#push)), work on your _local_ `main` is pushed onto the remote `main`, and is then represented on the `origin/main` locally.

At any point, you can point `origin/main` to another local branch of your choice. For example, if you work on `dev` locally and point it to `origin/main` using the command `git checkout -b dev origin/main`, then `git push` to remote, it will push your work to the remote `main` branch.

```
Example #1 - Init
      (local)                     (remote)
        C0                          C0
        |                           |
        C1 - *main & origin/main    C1
                                    |
                                    C2 - main

Example #2 - Pointing origin/main to dev, then pulling.
      (local)                     (remote)
        C0                          C0
        |                           |
        C1 - main                   C1
        |                           |
        C2 - *dev & origin/main     C2 - main

    git checkout -b dev origin/main
    git pull
```

In the example above, we create a new branch called `dev` and point it to follow `origin/main`. We then use the `git pull` function to update `origin/main`, and therefore `dev` as well. **The same is applicable for `git push`.**

In order to set remote tracking on a branch, we use the `git branch -u origin/main <branch name>` command. If `<branch name>` is blank, it will use the branch currently checked out.

### Fetch

`git fetch` allows you to retrieve data from the remote repository. It will download the missing commits on our local repository FROM the remote repository. It will then update where our remote branch points to.

```
Example #1 - Init

    (local)                             (remote)
    C0                                  C0
    |                                   |
    C1 - *main & origin/main            C1
                                        |
                                        C2
                                        |
                                        C3 - main

Example #2 - Using git fetch

    (local)                             (remote)
    C0                                  C0
    |                                   |
    C1 - *main                          C1
    |                                   |
    C2                                  C2
    |                                   |
    C3 - origin/main                    C3 - main
```

Slightly different layout in these examples. On the left-hand side, we have our local repositories, while on the right we have our remote repositories (hosted on Github for example).

If we only have our first two commits in our local repository, but the remote has two more (C2 & C3), then using `git fetch` allows us to retrieve `C2` & `C3` from remote. This will consequently update `origin/main`. However, `main` will remain in place.

**`git fetch` DOES NOT change your local state. It does not update your `main` branch or change your files. It simply downloads the missing ones.**

Fetch can optionally take some options: `git fetch <remote> <location>`. `<remote>` refers to the _local_ `origin/<branch>` branch in your repository where everything will be downloaded. `<location>` is the remote branch it will download from. It will download to `origin/<branch>` as to not mess up the files on your current `<branch>`, unless _explicitly_ stated using the `<source>:<destination>` refspec.

If you declare the `<location>`, then Git ignores where we are currently checked out. In the case where you would like to set the source AND destination (such as if you want to push to a secondary remote branch, not main), you can turn `<location>` into `<source>:<destination>`. For example: `git push origin feature:dev`. You can even use carets `^` and tilde `~` in the source, or create **new branches on the remote**! `<source>` refers to the location on the remote, while `<destination>` refers to the local place to put those commits.

In the case where `<source>` is empty, then that will create a local branch. For example: `git fetch origin :dev` will create a local `dev` branch.

### Pull

Fetching from the remote repository is crucial to having the most up-to-date files from remote. Once done, you can merge, rebase, cherry-pick, etc. `origin/main` at any point. OR....

Use `git pull`!

`git pull` is the short-form way of using a `git fetch` to update `origin/main`, followed by `git merge origin/main`. In both cases, you fetch the information from remote and update `main` to reflect those changes.

Pull can take the same arguments as `git fetch`. Doing so however will perform the `fetch` -> `merge` using the specified arguments. For example:

`git pull origin dev` is the same as saying:

```
git fetch origin dev
git merge origin/dev
```

Just like `git pull origin dev~1:feature` is the same as:

```
git fetch origin dev~1:feature
git merge feature
```

### Push

`git push` allows you to upload your changes to the remote repository. In other words, it publishes your work from your local repository to the remote one. Keep in mind that the behaviour of `git push` changes depending on your default settings.

When you use `git push`, it updates the remote repository with any outstanding commits it does not have, and points your _local_ remote branch (`origin/main`) to the most current commit as well to ensure **everything** is synced.

Push can optionally take some options: `git push <remote> <location>`. `<remote>` refers to the remote repository where everything will be pushed. `<location>` is where all the commits will come from. If you declare the `<location>`, then Git ignores where we are currently checked out. In the case where you would like to set the source AND destination (such as if you want to push to a secondary remote branch, not main), you can turn `<location>` into `<source>:<destination>`. For example: `git push origin feature:dev`. You can even use carets `^` and tilde `~` in the source, or create **new branches on the remote**!

In the case where `<source>` is empty, then that will delete the remote branch. For example: `git push origin :dev` will delete the remote `dev` branch.

Keep in mind that when HEAD is in detached mode (or not on a remote-tracking branch), then `git push` will fail.

#### Divergent History

When you're working alone on a repository, you manage all your files both locally and remotely. For the most part, everything should be synced no matter the circumstance.

What happens when MULTIPLE people are working on the same project? What if you `git pull` on a Monday, work on a feature throughout the week and try to `git push` on the Friday? Chances are, the remote repository has received several pushes from other users in the meantime.

In these cases, `git push` **will not** push your work to the remote repository.

One way we can accomplish the same with `git merge` instead.

```
Example #1 - Init
      (local)                         (remote)
        C0                              C0
        |                               |
        C1 - origin/main                C1
        |                               |
        C3 - *main                      C2 - main

Example #2 - Rebasing origin/main
      (local)                           (remote)
        C0                                  C0
        |                                   |
        C1                                  C1
       /  \                                /  \
      C3   C2                            C2    C3
      |   /                                \   |
      |  /                                  \  |
      C4 - *main & origin/main                C4 - main

        git fetch
        git rebase origin/main
        git push
```

We fetch the outstanding commits, and merge it into a new commit (`C4`), then applied the same to the remote. **This is the default way of solving divergent history when you `git pull; git push`**

Another way to resolve this kind of problem is to `git fetch` and then `git rebase`.

```
Example #1 - Init
      (local)                         (remote)
        C0                              C0
        |                               |
        C1 - origin/main                C1
        |                               |
        C3 - *main                      C2 - main

Example #2 - Rebasing origin/main
      (local)                         (remote)
        C0                              C0
        |                               |
        C1                              C1
       /  \                             |
     C3    C2                           C2
           |                            |
           C3' - *main & origin/main    C3 - main

        git fetch
        git rebase origin/main
        git push
```

When we `rebase origin/main`, we "reflect" the changes in the remote repository, then we push our changes. No conflicts!

**A shorthand form to accomplish this type of rebasing is `git pull --rebase`.**

### Protected Branches

In large collaborative projects, it's important to protect branches, especially if one of them is your "live" product (like `main`). You rarely ever want to push to `main` directly in these scenarios. Instead, create a branch which will then be merged into `main`. However, in the event you push to `main` by mistake, you may be greeted with an error which requests you to "pull first". To solve this:

Create another branch and push that to remote, and `git reset` `main` to be in sync with remote.
