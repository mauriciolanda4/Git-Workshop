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

