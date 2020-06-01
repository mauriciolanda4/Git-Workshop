# Git Workshop 

# Table of Contents
1. [Git cherry-pick](#git-cherry-pick)
2. [Git rebase](#git-rebase)
3. [Git alias](#git-alias)
4. [GitFlow](#gitflow)
5. [Git Commit Message Best Practices](#git-commit-message-best-practices)

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

### Git Commit Message Best Practices

#### Rules to consider

#### Make clean, single-purpose commits

This is to avoid getting sidetracked into doing too many things when working on one particular thing — when you are trying to fix one particular bug and you spot another one, and you cant resist the urge to fix that as well. And another one. Soon you end up with so many changes all going together in one commit.

- It makes it easier for other people in the team looking at your change, making code reviews more efficient.
- If the commit has to be rolled back completely, its far easier to do so.
- Its straightforward to track these changes with your ticketing system.

Additionally, it helps you mentally parse changes you've made using git log.

#### Write meaningful commit messages

Insightful and descriptive commit messages that concisely describe what changes are being made as part of a commit make life easier for others as well as your future self.

If you're using a ticketing system, you should also include the ticket id in the description. Several modern VCS hosting systems, such as GitHub, auto-link the commits to the issues.

#### Commit early, commit often

Git works best and works in your favor when you commit your work often. Instead of waiting to make the commit perfect, it is better to work in small chunks and keep committing your work. If you are working on a feature branch that could take some time to finish, it helps you keep your code updated with the latest changes so that you avoid conflicts.

Also, Git only takes full responsibility for your data when you commit. It helps you from losing work, reverting changes, and helping trace what you did when using git-reflog.

#### Don't alter published history

Once a commit has been merged to an upstream default branch (and is visible to others), it is strongly advised not to alter history. Git and other VCS tools to rewrite branch history, but doing so is problematic for everyone who has access to the repository. While git-rebase is a useful feature, it should only be used on branches that only you are working with.

Having said that, there would inevitably be occasions where there's a need for a history rewrite on a published branch. Extreme care must be practiced while doing so.

#### Don't commit generated files

Generally, only those files should be committed that have taken manual effort to create, and cannot be re-generated. Files can be re-generated at will can be generated any time, and normally don't work with line-based diff tracking as well. It is useful to add a .gitignore file in your repository's root to automatically tell Git which files or paths you don't want to track.

#### Format

<pre>< type >(< optional scope >): < subject >
< blank-line >
< optional body message >
< blank-line >
< optional body footer ></pre>

The first line should always be 50 characters or less and it should be followed by a blank line.

Often a subject by itself is sufficient. When it's not, add a blank line (this is important) followed by one or more paragraphs hard wrapped to 72 characters.

Those paragraphs should explain:

##### Why is this change necessary?

This answer explains what to expect in the commit, allowing them to more easily identify and point out unrelated changes.

##### How does it address the issue?

Describe, at a high level, what was done to affect change. If your change is obvious, you may be able to omit to address this question.

##### What side effects does this change have?

This is the most important question to answer, as it can point out problems where you are making too many changes in one commit or branch. One or two bullet points for related changes may be okay, but five or six are likely indicators of a commit that is doing too many things.

#### Avoid the use of the -m <msg> / --message=<msg> flag to git commit.

It makes it feel that you have to fit your commit message into the terminal command, and makes the commit feel more like a one-off argument than a page in history:

`git commit -m "Fix login bug"`

A more useful commit message might be:

<pre>Redirect the user to the requested page after login

https://trello.com/path/to/relevant/card

Users were being redirected to the home page after login, which is less
useful than redirecting to the page they had originally requested before
being redirected to the login form.

* Store requested path in a session variable
* Redirect to the stored location after successfully logging in the user</pre>

Consider including a link to the issue/story/card in the commit message a standard for your project in the message footer.

#### Type

Type of the made changes must be one of the following:

- build: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- ci: Changes to our CI configuration files and scripts (example scopes: Circle, BrowserStack, SauceLabs)
- docs: Documentation only changes
- feat: introduces a new feature to the codebase/modifies a current feature
- fix: Patches a bug in your codebase
- perf: A code change that improves performance
- refactor: A code change that neither fixes a bug nor adds a feature
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- test: Adding missing tests or correcting existing tests
- chore: Any other changes that don't affect code, such as changes to the build process or auxiliary tools and libraries
- improv: improves a current implementation without adding a new feature or fixing a bug

#### Scope

Scope of the changes specifying the place of the commit change, for example, $location, $browser, $compile, $rootScope, \$featureA, among others you can think of

You can use \* if there isn't a more fitting scope

#### Subject

The subject contains a very short description of the change:

- use the imperative, present tense: "change" not "changed" nor "changes"
- don't capitalize the first letter
- no dot (.) at the end

#### Body

Just as in the subject, use the imperative, present tense: "change" not "changed" nor "changes". The body should include the motivation for the change and contrast this with previous behavior.

#### Footer

All breaking changes have to be mentioned in the footer with the description of the change, justification and migration notes

Breaking Changes should start with the word BREAKING CHANGE: with a space or two newlines. The rest of the commit message is then used for this.

#### Referencing issues

Closed bugs should be listed on a separate line in the footer prefixed with "Closes" keyword like this:

`Closes #234`

or in case of multiple issues:

`Closes #123, #245, #992`

#### Important commit

you can add ! to the commit message to draw attention to breaking change

<pre>chore!: drop Node 6 from the testing matrix

BREAKING CHANGE: dropping Node 6 which hits the end of life in April</pre>

#### Examples

<pre>feat($browser): onUrlChange event (popstate/hashchange/polling)

Added new event to $browser:
- forward popstate event if available
- forward hashchange event if popstate not available
- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)</pre>

<pre>fix($compile): couple of unit tests for IE9

Older IEs serialize HTML uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately, jasmine does
not allow to user regexps for throw expectations.

Closes #392
Breaks foo.bar API, foo.baz should be used instead</pre>

<pre>feat(directive): ng:disabled, ng:checked, ng:multiple, ng:readonly, ng:selected

New directives for proper binding these attributes in older browsers (IE).
Added corresponding description, live examples, and e2e tests.

Closes #351</pre>

<pre>style($location): add couple of missing semi colons</pre>

<pre>docs(guide): update fix docs from Google Docs

A couple of typos fixed:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace</pre>

<pre>feat($compile): simplify isolate scope bindings

Changed the isolate scope binding options to:
  - @attr - attribute binding (including interpolation)
  - =model - by-directional model binding
  - &expr - expression execution binding

This change simplifies the terminology as well as
many choices available to the developer. It
also supports local name aliasing from the parent.

BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

scope: {
  myAttr: 'attribute',
  myBind: 'bind',
  myExpression: 'expression',
  myEval: 'evaluate',
  myAccessor: 'accessor'
}

After:

scope: {
  myAttr: '@',
  myBind: '@',
  myExpression: '&',
  // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
  myAccessor: '=' // in directive's template change myAccessor() to myAccessor
}

The removed `inject` wasn't generally useful for directives so there should be no code using it.</pre>

