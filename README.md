
# Git Style Guide

Git style guide.
This is personally how I use git,
which is similar to the Express and the iojs teams' style.

## Anti-Patterns

### `Merge branch 'master' into branch 'master'`

If you ever see a commit that merges a branch into itself,
just run `git rebase` and it will clean itself up.

### `Merge branch 'master' into branch ...`

If you're merging a parent branch into a branch and see this commit, you merged wrong.
Type `git rebase master` (or whatever the master branch is) to get rid of this commit.

## Merging

### Avoid pressing the big green button

If possible, don't click the big green "Merge Pull Request" button.
Doing so creates a `Merge branch...` commit, which is noisy.

### Add the "committed by" message to each merge

![](iojs-by.png)

- This is much cleaner than having commits that say "Merge Pull Request".
- All commits will be in the order in which they are committed.
  When you "merge" them, they are ordered by the date they are authored (technically no, but that's how it looks).
- It looks nice.

There are a couple of ways to add this:

- `git rebase` - only when you replay the commits, which is not always the case if the merged branch is already fast-forwarded.
- `git commit --amend`

### Amend the commits

So that they adhere to your styles:

- Any style changes.
- Change the commit message.
- Edit the commit description.
- Add any relevant links to the commit, such as links to the PR, issues, or information.
- Add a description.
- Add who else reviewed your commit.

![](commit-description.png)

### `git merge --squash`

When merging your own branch, you could use `git merge --squash` to squash all your commits.
I only say "merging your own branch" because doing so replaces the original author with you.
Thus, you are unable to properly assign blame.

### Rebase and merge

My personaly way of merging branches is the following:

Checkout the branch, rebase, and force push:

```bash
git checkout master
git pull

git checkout feature
# Maybe squash and amend
git rebase master
git push -f
```

If rebase does nothing, just do a `git commit --amend`.

The main reason is that you'd want the commits you merge to match the commits in the branch exactly.
This will tell GitHub to automatically close the pull request when you merge it like so:

```bash
git checkout master
git merge feature
git push
```

## Branching

### `git rebase master` all day every day

Don't diverge from your main branch as your problems will only exponentiate.

```bash
git rebase master
git push -f
```

### Squash frequently

Squash your commits frequently, but you should always squash it before merging.
You do not want or need your working commits (ex. a commit adding logs, another removing them)
in master as it's noisy and effectively negates itself.
Squash them out!

Another good reason to squash often is to make rebasing easy.
If you have a lot of commits, each commit is a potential merge conflict.
When you only have a single commit, you only need to do merge conflict resolution once!

### `git push -f` to only branches you own

Make sure you only push to the current branch!
There's a git config option for this, I don't remember what it was.
You do not want to overwrite shared branches unless everyone else knows!

## Committing

### Prefix the commit message with the submodule

For example, `readme: adding docs on stuff`.

### List which issues and PRs this commit solves

For example, `closes #56 closes #57`.
When the commit gets merged,
GitHub will know to automatically close these issues.
If you're working in a repository that is a fork or is expected to be a fork,
expand the issue like `closes jonathanong/git-style-guide#56`.

### Add emojis to your commit messages!

For example: 

> deps: :arrow_up: koa@~0.16.0

Set a standard for your team. 
Currently, I only do arrows for dependency changes.
It makes git way more fun!
