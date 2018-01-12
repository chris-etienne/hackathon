# Git Reference

This guide is a quick introduction to using Git. The most common commands are
 described for reference. This guide's examples uses the [lagom-evolution on Bitbucket](https://bitbucket.org/nx-team/lagom-evolution/src)
 repository using the [command line tool](https://help.github.com/articles/set-up-git/#setting-up-git). 

## Overview

`pull requests,` commonly referred to as `PRs,` are how changes to code repositories are proposed.
  The `pull request` request is a set of proposed changes. The owner of repository will review
  the request and either `merge` your PR or request changes to your PR.
  
When URL's are specific to your account, you'll want to replace the `<YOUR NAME HERE>` in the example URL 
 with your Bitbucket username.

### Authentication

You can authenticate with `Bitbucket` via `https` or `ssh`. To use ssh, recommended, add a public key
 to your bitbucket user account settings, (`ssh-keys`)[https://bitbucket.org/account/user/<YOUR NAME HERE>/ssh-keys/]. 
 
### Fork a Repository 

The first step in creating a PR is to `fork` the repo that you want to change.

A fork is a copy of a repository. Forking a repository allows you to freely
 experiment with changes without affecting the original project. Commits pushed to your
 fork are _not_ seen in the main repo until your PR is `merged` into the main repo by the repo
 owner.

From the [project](https://bitbucket.org/nx-team/evolution/overview) page on BitBucket,
 use the `create` (the "+" icon under search on the left-hand menu).
 To access the `Fork this repository` under `Get to Work` of the create pop-up menu.

See also: [Github Doc - Forking](https://help.github.com/articles/fork-a-repo/)

### Clone your Fork

Next, you want to `clone` your fork to your local development environment.
 From the `overview` page of _your_ fork, copy the `SSH` URL immediately under `Overview`.
 If you not provided a public SSH key to your Bitbucket account, you'll need to choose
 the `HTTPS` URL format.

If you have trouble locating your fork, view the `Forks`
 list from the main repo's (overview page)[https://bitbucket.org/nx-team/evolution/overview].
 
`clone` your repo from the directory that you want the source tree to be cloned into. If you clone
  from within `~/src`, cloning the top level of Evolution will be in `/src/evolution`

``` 
git clone git@bitbucket.org:<YOUR NAME HERE>/evolution.git
```

### Add the Upstream Remote

When a repo is cloned, it has a default remote called `origin` that points to your
 fork on GitHub, _not_ the original repo it was forked from.
To keep track of the original repo, you need to add the original repo as a `remote.`
By convention, `upstream` refers to the original repo that you have forked.

Obtain the upstream URL from the `overview` page of the
 [Evolution Project](https://bitbucket.org/nx-team/evolution/overview).
 Add this URL as a remote named `upstream` to your repo.

```
git remote add upstream git@bitbucket.org:nx-team/evolution.git
```

Next, fetch the upstream to make its branches available to your local repo.
```
git fetch upstream
```

Confirm the remote addition
```
git remote -v
```

Expected output
````
origin	git@bitbucket.org:<YOUR NAME HERE>/evolution.git (fetch)
origin	git@bitbucket.org:<YOUR NAME HERE>/evolution.git (push)
upstream	git@bitbucket.org:nx-team/evolution.git (fetch)
upstream	git@bitbucket.org:nx-team/evolution.git (push)
````

See also:
 [Stack Overflow - What is the difference between origin and upstream on GitHub?](https://stackoverflow.com/questions/9257533/what-is-the-difference-between-origin-and-upstream-on-github#9257901)

### Create a Branch

To create a new branch, use `git checkout -b`
```
 git checkout -b myBranch-IssueXYZ
```

To checkout an existing branch, use 'git checkout'
```
git checkout master
```

To view branches, use `git branch -a`
``` 
git branch -a
* master
  myBranch-IssueXYZ
  remotes/origin/HEAD -> origin/master
  remotes/origin/master

```

To checkout a branch from a different remote, specify the remote and branch
```
 git checkout origin/master
```

### Syncing a fork

You should update your fork before creating new branches and before submitting a pull request.
 You can use `merge` or `rebase` for this. Both start by fetching the latest changes from the upstream.  

To merge, merge up upstream master into your working branch:

```
git fetch upstream
git merge upstream/master
```

Using an interactive rebase:

```
git fetch upstream
git rebase -i origin/master
```

This will open up a screen in your editor, allowing you to say what should be done with each commit.
 If the commit message for the first commit is suitable for describing all of the commits, then leave it as is,
 otherwise, change the command for it from pick to reword

For each remaining commit, change the command from `pick` to `fixup`. This tells git to merge that commit into the
 previous commit, using the commit message from the previous commit.

See also:
  * [GitHub Doc - Syncing a Fork](https://help.github.com/articles/syncing-a-fork/)
  * [github doc](https://help.github.com/articles/syncing-a-fork)
  * [playframework doc](https://playframework.com/documentation/2.6.x/WorkingWithGit)

### Push a Branch 
 
To propagate your local commits to the server, use `git push`. 

```
git push origin
```

### Squash your commits

If your branch as multiple commits, you can `squash` your commits into as single commit using an interactive rebase.
 For example, to squash 3 commits, `A`, `B` and `C`, into one, start an interactive rebase with:

```
git rebase -i HEAD~3
```

Will launch your editor with the three commits set to `pick`. Note that the latest commit will be at
 the _bottom_ of the presented list.
```
pick C
pick B
pick A
```

Change `pick` for `A` and `B` to `squash` these newer commits into the older `C` commit.
 Save and quit the editor. A second editor view will be used to specify the commit message.
 
An alternative approach to squashing commits is to undo your commits and re-commit them into a single commit.
 Continuing the above example, reset your working copy `HEAD` to 3 commits ago:
```
git reset HEAD~3
``` 

Commits `A`, `B` and `C` have been undone. You can now re-add your changes, and re-commit them as a single commit.

See also:
 * [SO - Squash two commits](https://stackoverflow.com/questions/2563632/how-can-i-merge-two-commits-into-one)
 * [GitReady - Squash with Rebase](http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)

### Creating a PullRequest

After you have committed and pushed your changes, you can create a pull request. Choose 'Create a Pull Request' from
the "+" create menu to get to the `pull-requests/new` page. Choose the branch you want to changes to come from and what
remote and branch you want changes to go to. Normally this will the `master` of `upstream`. Review your commits and diffs,
and provide a description of the changes for the reviews. Specify reviewers, if appropriate, and create your pull request.

Reviewers may request additional changes to your PR. You can push additional commits into the working branch for this.
 Don't forget to rebase your branch if needed.git 

### Branching a Release Branch

To PR a release branch, use the upstream release branch as your start point.
 In this example, we create a branch called `fix-v1.0` using the `upstream/1.0` branch,
 the `1.0` release branch on the upstream, as the start point.
 
``` 
git checkout -b fix-v1.0 upstream/1.0
```

## Test a PR locally

To test a Pull Request locally before merging the PR, pull the PR into a test branch.

From your project repository, check out a new branch and test the changes.

```
git checkout -b the-name-of-your-test-branch master
git pull git@bitbucket.org:<PR Sumbitter User>/evolution.git the-name-of-your-test-branch
```

Your test branch will now have the PR changes locally. Perform your testing.
If you are satisfied with the PR, merge the PR using the UI or from the command line:

```
git checkout master
git merge --no-ff jthe-name-of-your-test-branch
git push origin master
```

## Additional Resources

[Git Cheat Sheet](http://www.ndpsoftware.com/git-cheatsheet.html)
[Git Immersion](http://gitimmersion.com/)
