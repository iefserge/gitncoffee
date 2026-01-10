# Patches

This document describes a Git patch-based workflow based on branches.
It's a simple model for code submissions and review, but with some useful features.
It's an alternative to pull request workflow. Here, we also call it Git ‘n’ Coffee workflow.

*TLDR: In patch workflow:*

- *Git push to submit code and create a patch (no email or UI clicks)*
- *Each push is snapshotted and versioned*
- *Branch-level access control, repository forks are not used*
- *Patches can be stacked*
- *Quick integration of patches with `git am`*

## Introduction

A patch is a unit of code change, a diff, or a comparison between two versions.
A patch describes files and lines to modify, insert, or delete. It's a human
and machine-readable representation of change.

Traditionally, patches are associated with email, but Git ‘n’ Coffee makes
it possible to use regular Git branches to submit and apply patches. You can
send a patch by running `git push`, over the standard Git protocol. And no email is necessary.

While the code submission process is the same as with pull requests,
the difference is that incoming commits are processed in a special way.
For example, commit hashes are mostly ignored, and commits themselves are
sorted into one or many patches.

This model opens up some interesting possibilities, which may be harder to
achieve with conventional pull requests:
- Commit post-processing. For example, automatic code formatting on push.
- Stacked diffs without multiple branches.
- Change-ID support and versioning of each patch.

Patches are also created and updated in response to `git push` and without any UI interaction.

Git ‘n’ Coffee was inspired by workflows used in large open source
projects like the Linux kernel and Git itself. These projects primarily use patches rather
than pull requests.

## Patch Versioning

In many cases when making changes, we are not only interested in the final result,
but also in intermediate steps. Or, we may be working on something that doesn't
have an obvious solution, so multiple approaches need to be tried. In this process,
we're modifying our solution, producing new versions every time. Then our thought process
and reasoning is captured in those intermediate steps.

This is patch versioning. Each intermediate step is its own patch and a version, that is
snapshotted and captured the moment it was created. Git ‘n’ Coffee stores each
version and allows comparison between versions.

Pull request systems represent versions as commits on a branch, but this model quickly
breaks down if commit history is rewritten.

Another important difference is that commit hashes are also ignored when versioning patches.
Instead, Git ‘n’ Coffee compares the actual content, so changing just
the hash won't create a new version.

## Patch Stacks

Large changes are hard to review, so we'd like to break them down into smaller pieces.
A patch stack represents a series of patches, which build on top of each other.

It's worth noting here that each patch is also versioned separately, so making a change
to one patch in the stack doesn't update other patches in that stack. Since commit
hashes are changing on each rebase, the actual content comparison is particularly important here.

## Integrating Patches

Patches are applied (or cherry-picked) on top of previous commits, and there are no
merge commits or joined histories. Each patch produces exactly one commit, and includes
its full description in the commit message. This is similar to "squash and merge"
merge strategy of pull requests and achieves a similar result.

When integrating patch stacks, we get multiple commits, one per patch in the stack.

Multiple patches can be applied to the same branch at the same time. For example, you
can apply a set of patches to your branch while doing other work on the same branch. This
can be more flexible than managing multiple pull request branches.

## Pull Requests and Forks

When contributing to public repositories, pull request systems often require that the
repository is first "forked", then a pull request between branches in two repositories
is created.

Git ‘n’ Coffee doesn't require forks. In public repositories, patch branches can be
pushed without commit permissions. This is possible because Git ‘n’ Coffee manages
permissions on a branch level too, and not only on a repository level. The only
requirement is that patch branches are named in a special way.

This makes it easy to submit a patch to any open-source project that accepts contributions.

## Submitting a Patch

To submit a patch, create a new branch that starts with `patch/`, make changes, and push it.
Git ‘n’ Coffee will combine all commits on this branch and turn them into a single patch.
If you're not working with patch stacks, this is the simpler way to submit a single patch.

Example:

```bash
git clone <repository>
git switch -c patch/my-fix
# make and add changes
git commit
git push
```

If your commit message includes a longer description, it will be rendered using markdown.
When patch is applied, full description will be preserved in the commit log.

When you push a patch branch, Git ‘n’ Coffee assigns branch ownership to you.
After that, only you (and repository maintainers) can update that branch.

To update a patch, push more commits to the same branch. You can add commits, amend them,
or rewrite history. Git ‘n’ Coffee will keep track of each version. Reviewers can then
view and compare previous versions of the patch as it evolves.

## Working with Patch Stacks

If your work involves a series of related changes, you can submit them as a patch stack.
To do that, create a branch that starts with `patchstack/` and make multiple commits.
Each commit will become a separate patch in the stack.

Example:

```bash
git clone <repository>
git switch -c patchstack/my-feature
git commit -m 'Part 1: Add helpers'
git commit -m 'Part 2: Use helpers in logic'
git push
```

To update the stack, you can reorder, squash, or amend commits and then force-push.
Git ‘n’ Coffee will match the updated commits to the original ones and will update patches as necessary.

You can also use git rebase's `--fixup` flag to quickly tweak earlier commits in your stack.
For convenience, Git ‘n’ Coffee provides a copy-pasteable command to do this in the UI.

Patches in the patch stack can also be added or removed by adding or removing commits.

## Applying a Patch

Git command-line interface includes `git am` command which applies patches to the current branch.
You can combine it with `curl` to apply any patch from Git ‘n’ Coffee to your current branch.

Git ‘n’ Coffee UI provides a link to a `*.patch` URL for each patch and a one-liner git command
to apply it locally. In many cases, this is faster than fetching code and merging a branch.

## Code Review

Each patch and each version of each patch can be reviewed independently, or by different people.

If you're reviewing incoming patches, Git ‘n’ Coffee provides a few useful tools:

* You can add comments to the whole patch or individual lines.
* You can quickly archive patches that don't meet your project's standard.
* You can apply patches locally with one command.
* You can accept patches once they're reviewed or use "Accept with Changes" if you're okay
  with the author making final tweaks.

_Note_: Git ‘n’ Coffee doesn't apply or merge anything automatically yet. If a patch is accepted,
it still needs to be applied manually and pushed to the main branch.

## A note on JJ

Jujutsu is a version control system that supports Git as a backend. This is still work in
progress, but special support is planned for `*/push-*` branches.

## Alternative patch workflows

Here are some tools and services that implement patch-based or stacked branch workflows
on top of GitHub pull requests, GitLab merge requests or other Git hosting solutions.

Listed in sorted order:

* [GitButler's new patch based Code Review (Beta)](https://blog.gitbutler.com/gitbutlers-new-patch-based-code-review/)
* [Graphite](https://app.graphite.dev/)
* [Mergify Stacks](https://docs.mergify.com/stacks/)
* [ReviewStack](https://sapling-scm.com/docs/addons/reviewstack/)
* [Reviewable](https://www.reviewable.io/)
* [Stacked Git, StGit](https://stacked-git.github.io/)
* [ghstack](https://github.com/ezyang/ghstack)
* [git-branchless: Branchless workflow for Git](https://github.com/arxanas/git-branchless)
* [git-pr](https://pr.pico.sh/)
* [git-ps - Git Patch Stack](https://git-ps.sh/)
* [git-spice](https://abhinav.github.io/git-spice/)
* [git-stack: Stacked branch management for Git](https://github.com/gitext-rs/git-stack)
