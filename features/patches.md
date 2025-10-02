# Patches

This document describes a Git patch-based workflow built for Gitpatch. It's a simple model for code submissions and review, but with some advanced features. It's an alternative to pull request workflows. Here, we call it Gitpatch workflow.

*TLDR: In Gitpatch workflow:*

- *Git push to submit code and create a patch (no email or UI clicks)*
- *Each push is snapshotted and versioned*
- *Branch-level access control, repository forks are not used*
- *Patches can be stacked*
- *Quick integration of patches with `git am`*

## Introduction

A patch is a unit of code change, a diff, or a comparison between two versions. A patch describes what files to change and lines to modify, insert, or delete. We can think of a patch as a basic building block for more complex concepts like pull requests, patch stacks, or interdiffs (i.e. diff between two diffs).

When working with code and version control systems, we see patches everywhere, in our diff viewers, in pull requests, and even as outputs produced by code assistant LLM tools. But the idea is the same: it's a human and machine-readable representation of change.

Traditionally, patches are associated with email, but Gitpatch makes it possible to use regular Git branches to submit and apply patches. So you can send a patch by running `git push`, over the standard Git protocols. And no email is necessary.

One major point here is that while the code submission process is pretty much the same as with pull requests, the difference is that incoming commits are processed in a special way. For example, commit hashes are completely ignored, and commits themselves are, depending on the branch type, sorted into one or multiple patches.

This model opens up some interesting possibilities, which are harder to achieve with pull requests:
- Commit post-processing. For example, automatic code formatting on push.
- Built-in support for fix-up commits. Certain commits can be auto-squashed into other commits.
- Patch stacking without multiple branches.
- Change-ID support.

Patches are also created automatically in response to a git push, without any UI interaction.

Gitpatch was inspired by the workflows used in large open source projects like the Linux kernel and Git itself. These projects primarily use patches rather than pull requests.

## Patch Versioning

In many cases when making changes, we are not only interested in the final result, but also in intermediate steps. Or, we may be working on something that doesn't have an obvious solution, so multiple approaches need to be tried. In this process, we're modifying our solution, producing new versions every time. Then our thought process and reasoning is captured by those intermediate steps.

This is patch versioning. Each intermediate step is its own patch and a version, that is snapshotted and captured the moment it was created. Gitpatch stores each version and allows comparison between versions.

Pull request systems represent versions as commits on a branch, but this model quickly breaks down if commit history is rewritten.

Another important difference is that commit hashes are not taken into the account when versioning patches. Instead, Gitpatch compares the actual content, so changing just the hash won't create a new version.

## Patch Stacks

Large changes are hard to review, so we'd like to break them down into smaller pieces. A patch stack represents a series of patches, which build on top of each other.

It's worth noting here that each patch is also versioned separately, so making a change to one patch in the stack doesn't update other patches in that stack. Since commit hashes are changing on each rebase, the feature of only comparing the actual content is particularly important here.

## Integrating Patches

Patches are applied (or cherry-picked) on top of previous commits, and there are no merge commits or joined histories. Each patch produces exactly one commit, and includes its full description in the commit message. This is similar to "squash and merge" merge strategy of pull requests and achieves a similar result.

When integrating patch stacks, we get multiple commits, one per patch in the stack.

Multiple patches can be applied to the same branch at the same time. For example, you can apply a set of patches to your branch while doing other work on the same branch. This is more flexible than fetching and merging pull request branches, which are often not versioned, contain too many fix-up commits, and can be hard to review.

## Pull Requests and Forks

When contributing to public repositories, pull request systems often require that the repository is first "forked", then a pull request between branches in two repositories is created. This adds extra steps, making contributions more difficult.

Gitpatch doesn't need nor require forks. In public repositories, patch branches can be pushed without commit permissions. This is possible because Gitpatch manages permissions on a branch level too, and not only on a repository level. The only requirement is that patch branches are named in a special way.

This makes it easy to submit a patch to any open-source project that accepts contributions.

## Submitting a Patch

To submit a patch, create a new branch that starts with `patch/`, make changes, and push it. Gitpatch will combine all commits on this branch and turn them into a single patch. If you're not working with patch stacks, this is the simpler way to submit a single patch.

Example:

```bash
git clone <repository>
git switch -c patch/my-fix
# make and add changes
git commit
git push
```

If your commit message includes a longer description, it will be rendered using markdown. When patch is applied, full description will be preserved in the commit log.

When you push a patch branch, Gitpatch assigns branch ownership to you. After that, only you (and repository maintainers) can update that branch.

To update a patch, push more commits to the same branch. You can add commits, amend them, or rewrite history. Gitpatch will keep track of each version. Reviewers can then view and compare previous versions of a patch as it evolves.

## Working with Patch Stacks

If your work involves a series of related changes, you can submit them as a patch stack. To do that, create a branch that starts with `patchstack/` and make multiple commits. Each commit will become a separate patch in the stack.

Example:

```bash
git clone <repository>
git switch -c patchstack/my-feature
git commit -m 'Part 1: Add helpers'
git commit -m 'Part 2: Use helpers in logic'
git push
```

To update the stack, you can reorder, squash, or amend commits and then force-push. Gitpatch will match the updated commits to the original ones and will update patches as necessary.

You can also use git rebase's `--fixup` flag to quickly tweak earlier commits in your stack. For convenience, Gitpatch provides a command to do this in the UI.

Patches in the patch stack can also be added or removed by adding or removing commits.

## Applying a Patch

Git command-line interface includes `git am` command which applies patches to the current branch. You can combine it with `curl` to apply any patch from Gitpatch to your current branch.

Gitpatch UI provides a link to a `*.patch` URL for each patch and a one-liner git command to apply it locally. In many cases, this is faster than fetching code and merging a branch.

## Code Review

Each patch and each version of each patch can be reviewed independently, or by different people. 

If you're reviewing incoming patches, Gitpatch provides a few useful tools:

* You can add comments to the whole patch or individual lines.
* You can quickly archive patches that don't meet your project's standard.
* You can apply patches locally with one command.
* You can accept patches once they're reviewed or use "Accept with Changes" if you're okay with the author making final tweaks.

_Note_: Gitpatch doesn't apply or merge anything automatically yet. If a patch is accepted, it still needs to be applied manually and pushed to the main branch.

## A note on JJ

Jujutsu is a version control system that supports Git as a backend. This is still work in progress, but special support is planned for `*/push-*` branches.
