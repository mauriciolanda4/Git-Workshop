# Git Workshop 

### Git cherry-pick

#### What does cherry-pick do?
This command allows you to take one or more commits from another branch without having to make a full merge.

In case you made a mistake and committed a change into the wrong branch, but do not want to merge the whole branch. You can revert the commit and apply it on another branch.

#### Why cherry-pick?
Suppose you are working with a team of developers on a medium to large-sized project. Some changes proposed by another team member and you want to apply some of them to your main project, not all. Since managing the changes between several Git branches can become a complex task, and you don't want to merge a whole branch into another branch. You only need to pick one or two specific commits. To pick some changes into your main project branch from other branches is called cherry-picking.

If you need to add several consecutive commits, it is better to use merge or rebase.

#### How to use cherry-pick?
1.- Make sure you are on the branch you want to apply the commit to(not the one with the commit).

```git checkout master```

2.- Each commit has a unique hash (which looks something like 2f5451f).

You need to find the hash for the commit you want to cherry-pick.
Here are two places you can see the hash for commits:

- In the commit history on GitHub/GitLab/Bitbucket.
- In your terminal (Terminal, Git Bash, or Windows Command Prompt) run the command ```git log --oneline```.
 
3.- Execute the following:
```git cherry-pick <commit-hash>```

If the cherry picking gets halted because of conflicts, resolve them and:
```git cherry-pick --continue```

If you dont want to continue with the cherry-picking, use:
```_git cherry-pick --abort```

4.- Push to target branch.

#### How to cherry-pick multiple commits
You can cherry-pick multiple commits by listing multiple hashes in the order they were originally committed.

```git cherry-pick <commit-hash-1> <commit-hash-5> <commit-hash-7>```

### Git rebase

#### What is git rebase?
Rebase re-writes the changes of one branch onto another _without_ creating a new commit.

#### How does rebase works?
Rebasing takes your commits -> store it in a temporary location -> Take the latest version of your repo -> add your commits on top of the latest version available. So in this way Rebasing ensures that your commits are added to the current state of the project and not to the old state of the project which was there when you cloned the project.

For every commit that you have on the feature branch and not in the master, a new commit will be created on top of the master. It will appear as if those commits were written on top of master branch all along.

##### Rebase pros and cons
Pros:
- clean and flat history.
- Concise commits covering a logical change(can squash series of commits into one).
- Reduction of merge commits.
- Manipulating single commits is easy (e.x. reverting a commit).

Cons
- More complex.
- Can cause history rewrite if done incorrectly.

#### How to not screw up a rebase
- _Never use an IDE to perform rebase (or Git) tasks for you_.
- Do not rebase any public or group branches, if you need to fix problems on a public or group branch, use a commit instead.
- _Make sure that your working tree is clean._ Before rebasing, check ```git status```
- _Work on a branch in only one terminal/window._ Git maintains per repository locks and internal state that can become inconsistent if you’re modifying things from multiple terminals.
- _Don’t make any changes to files in your branch until your rebase is complete_ unless you have to, for example, to fix a conflict.
- Don’t be afraid to abort if you get in over your head. Abort your failed attempt, learn more and then come back and try again. Aborting puts things back the way they were. 

#### How to use Rebase in feature branches
The following is an example workflow for developing on a temporary branch and merging back to the main branch squashing all commits into a single commit.  This assumes you already have a branch named ``branch-xyz`` and have finished the work on that branch.
##### Step 1: Checkout the feature branch
```
git checkout branch-xyz
```
##### Step 2: Rebase the branch to the master branch
This will replay all commits on the feature branch on top of the latest code from the master branch.  This makes it look like the feature branch was copied from the master branch and all commits laid on top, even if other commits happened on the master.  This makes merging later on easy.  This assumes that master is up to date with your remote repository.  If not, make sure to do a ``git pull`` on the master branch to bring it up to date.
```
git rebase master
```
##### Step 3: Resolve conflicts
If any conflicts occurred while rebasing, you will need to resolve those conflicts before proceeding.  To resolve the conflicts, open the conflicting files in an editor and update the demarked lines.  You  may need to repeat these steps as necessary until all rebasing has been completed.
```
[update file1 file2 etc]
git add file1 file2 etc
git rebase --continue
```
##### Step 4: Checkout master
At this point, the feature branch is up to date with master, so we can checkout master again to get ready to merge.
```
git checkout master
```
##### Step 5: Merge the feature branch
We can now merge the feature branch into master squashing all commits into a single commit.  This allows you to commit early and often on the feature branch and only update the main code base as if only one commit happened keeping a nice and clean history.
```
git merge --squash branch-xyz
```
##### Step 6: Commit
At this point you can commit the merged updates.  When you commit, it will give you a prefilled commit message containing all the merged commits.  You can simply delete those lines and create a new line that describes the update or the bug fix.  Optionally, you can choose to push the commits back to the remote repository.
```
git commit
git push
```
##### Step 7: Finish
You are now done.  You can choose to delete the branch, although deleting the branch will remove all the individual logged commits on the branch since they were squashed on the master branch.

### Git alias

#### What is git alias?
The term alias is synonymous with a shortcut.

Aliases are created through the use of the ```git config``` command and the Git configuration files. As with other configuration values, aliases can be created in a local or global scope.

To set up a new alias for a command, we need to use the git config command.

Structure: git config --global alias.$alias_name $command

```git config --global alias.c checkout```

##### How do i display all my aliases?
Run this command in your terminal:

```git config --global alias.alias "! git config --get-regexp ^alias\. | sed -e s/^alias\.// -e s/\ /\ =\ /"```

Now you can use `git alias` to list all the aliases you have created.

##### How do i configure external commands outside of git?
With the use of `!` at the start of the command, for example:

To open VS Code:

```git config --global alias.code "! code ."```

##### Where do i access my git config file?
The global or local config files can be manually edited and saved to create aliases. The global config file lives at \$HOME/.gitconfig file path. The local path lives within an active git repository at /.git/config.

Using VIM in the terminal:

```vim ~/.gitconfig```

##### How do i execute external commands?
With the use of `!` at the start of the command and ```&&```, for example:

To checkout to master and pull:

`git config --global alias.mp "! git checkout master && git pull"`

##### My list of git aliases
```
	alias = ! git config --get-regexp ^alias\\. | sed -e s/^alias\\.// -e s/\\ /\\ =\\ /
	c = commit
	cm = commit -m
	a = add --all
	acm = ! git add . && git commit -m
	c = clone
	s = status
	b = checkout -b
	m = checkout master
	d = branch -D
	mp = ! git checkout master && git pull
	l = log --graph --oneline --decorate --all
	ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate
	ll = ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat
	sta = stash apply
	st = stash
	unstage = reset HEAD --
	code = ! code .
```

### GitFlow 

Git-flow is a branching model for Git, which makes it easier for us to work with our repositories.
It's simply a set of scripts that bundle Git commands into workflows.

## Key Benefits

#### Parallel Development

One of the great things about GitFlow is that it makes parallel development very easy, by isolating new development from finished work. New development (such as features and non-emergency bug fixes) is done in feature branches and is only merged back into the main body of code when the developer(s) is happy that the code is ready for release.

Although interruptions are a bad thing, if you are asked to switch from one task to another, all you need to do is commit your changes and then create a new feature branch for your new task. When that task is done, just checkout your original feature branch and you can continue where you left off.

#### Collaboration

Feature branches also make it easier for two or more developers to collaborate on the same feature, because each feature branch is a sandbox where the only changes are the changes necessary to get the new feature working. That makes it very easy to see and follow what each collaborator is doing.

#### Release Staging Area

As new development is completed, it gets merged back into the develop branch, which is a staging area for all completed features that haven’t yet been released. So when the next release is branched off of develop, it will automatically contain all of the new stuff that has been finished.

#### Support For Emergency Fixes

GitFlow supports hotfix branches - branches made from a tagged release. You can use these to make an emergency change, safe in the knowledge that the hotfix will only contain your emergency fix. There’s no risk that you’ll accidentally merge in a new development at the same time.

#### Main Branches

Branches that exist forever on a repository

- Master (Represent production-ready state of source code). To this branch, only code that is in - or is ready to be in - production should arrive.
- Develop (Represent Latest delivered development changes or also called integration branch). All the developments we make, new modules, refactorings, etc. will be stored in Develop, waiting to go to production.

It is strictly forbidden for devs to write on those two branches.

#### Supporting Branches

Branches that once completed their function are deleted.

- Feature branches (feature/???)
- Release branches (release/???) e.g release-\*
- Hotfix branches (hotfix/???) e.g hotfix-\*

??? It depends upon the naming convention you prefer or anything that is preferred by your organization.

#### Feature Branches

##### May branch off from:develop

##### Must merge back into:develop

- Each new feature should reside in its own branch.
- Also sometimes called topic branches.
- Merge back to develop when we want to add the new feature to the upcoming release if the feature is unstable/incomplete do not merge feature in to develop branch.
- Typically exist in developer repos only, not in origin.
- Use --no-ff flag to merge changes in to develop branch to keep track of the historical existence of feature branches
- Discard in case of a bad experiment.

#### Release Branches

##### May branch off from:develop

##### Must merge back into:develop and master

- Represent preparation of a new production release to deploy changes on the Testing server.
- Do bug fixing in this branch, bug fixes may be continuously merged back into the develop branch.
- In this branch Adding large new features here is strictly prohibited.
- All features that are targeted for the release must be merged into develop.
- Create a release branch with a name reflecting the new version number.
  e.g (git checkout -b release-1.7 develop).
- If the release branch is ready to be released on the production server do the following steps.
  1. Merge release branch into master.
  2. Create a tag for future reference, e.g. (1.7).
  3. Merge changes back to develop branch.
  4. Delete release branch.

#### Hotfix Branches

##### May branch off from:develop

##### Must merge back into:develop and master

-Hotfix branches are created from the master branch.
(e.g) git checkout -b hotfix-1.7.1 master.

- Fix the bug in hotfix branch, when finished with bug fixing perform the following steps:
  1. Merged hotfix branch into master.
  2. Create Tag for future reference e.g (1.7.1).
  3. Merge changes back to develop branch (Section Exception Case).
  4. Delete hotfix Branch.

#### Exception Case

The one exception to the rule here is that, when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of develop. Back-merging the bug fix into the release branch will eventually result in the bug fix being merged into develop too when the release branch is finished. (If work in develop immediately requires this bug fix and cannot wait for the release branch to be finished, you may safely merge the bug fix into develop branch).

#### Overall Flow

- A develop branch is created from master.
- A release branch is created from develop.
- Feature branches are created from develop.
- When a feature is complete it is merged into the develop branch.
- When the release branch is done it is merged into develop and master.
- If an issue in master is detected a hotfix branch is created from master.
- Once the hotfix is complete it is merged to both develop and master.

#### Branch name conventions

Branch names should contain only **lowercase letters**, **numbers** and **hyphens**, regexp: /[a-z0-9\-]+/.

Why not with slashes? because of continuous deployment to the corresponding subdomain, for example, **feature-ui-auth** will be deployed to **ui-auth.project .com**, this is why we should use only symbols allowed for DNS names.

