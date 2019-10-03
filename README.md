# Block fixup commit merge Action

Love using `git commit --fixup` to make minor cleanups to your Git history, but
forget to `git rebase -i --autosquash master` before merging?

Have I got a Github Action for you!

## Setup

Create a file in your project named: `.github/workflows/git.yml` Add the following:

```yaml
name: Git Checks

on: [push]

jobs:
  block-fixup:
    runs-on: ubuntu-18.04

    steps:
    - uses: actions/checkout@master
    - name: Block Fixup Commit Merge
      uses: 13rac1/block-fixup-merge-action@v1.0.0
```

Optionally, setup Branch Protection to block merging of PRs against the `master`
branch with `!fixup` commits.

[Example PR blocked by a `fixup!` commit:][example-pr]

[example-pr]:https://github.com/13rac1/block-fixup-merge-action/pull/1
[![PR merge
blocked](images/block-fixup-example.png?raw=true)](images/block-fixup-example.png?raw=true)

## Why

Change Requests are part of a Pull Request code review. There are multiple
methods to make the updates, some are better than others. The regular method is
to add additional fix/update commits, including commit message titles such as
"Code Review changes." There's a better way.

Git commits are the historical record of project. The commit messages tell a
story, each commit or Pull Request describing a change. FutureYou™ and
CurrentYouCoWorkers™ needs to know the end result of each change, but no one
cares about edits required to make that change. We just want to read the book, no one cares what the editor told the author while writing the book.

One solution is to use Github's "Squash merge" feature. This works well for
small PRs. A few small commits can be squashed into a single commit. The "edits"
removed.

The problem occcurs when you have multiple unique commits a PR: a dependency
update, followed by a refactor, then a bug fix. It is all one contained change,
but FutureYou™ may just want to see that bug fix. Keep it separate. A solution
is using `git rebase` to edit commits during the code review; this becomes
confusing for reviewers, commits can suddenly disappear. "Fixup Commits" are
better.

Fixup Commits can added to the end of the git history during a code review, then
squashed into their respective commits after the PR is approved. The workflow
is:

1. Make a feature branch.
2. Add commits implementing features and fixing bugs. Rebase and force push all
   you want for now. This is _your_ branch.
3. Create a Pull Request and add Reviewer(s.) Rebasing commits is not allowed
   for now. This is not _just your_ branch.
4. Reviewer(s) request changes.
5. Create commits using `git commit --fixup=<SHA>` to prepare changes to
   existing commits. Make new commits only when the change is outside the scope
   of existing changes.
6. Reviewers see new commits and approve the PR.
7. This is _your_ branch again. Clean up the history to make an ideal set of
   commits `git rebase -i --autosquash`, and merge.

Note: Rebasing is still allowed for the entire branch to integrate `master`
branch changes as needed.

## License

MIT
