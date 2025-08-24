# Gitpatch Docs

**Gitpatch** is a cloud-based code hosting service built to help asynchronous teams collaborate better using Git. Instead of pull requests, it uses patches, which are smaller units of change.

Patches, compared to pull requests, are lighter weight, easier to review, and quicker to apply. They are versioned and can be stacked on top of each other. And there is no email: patches are submitted using the standard Git protocol.

Gitpatch is also built to be fast. Like, really fast. Under the hood, Gitpatch implements a custom Git storage system and additional indexing to speed up many operations.

---

## How It Works

Gitpatch is like any other Git remote. You can clone repositories, make changes, and push branches. The main feature is that if a branch name starts with `patch/` or `patchstack/`, Gitpatch will handle it in a special way and will turn those commits into one or more patches.

In public repositories, patch branches can be pushed without commit permissions to the main repository. This makes it easy to submit a patch to any project that accepts contributions; no repository forking is needed. Organizations and support for private repositories are still in development.

## How Are Patch-Based Workflows Different?

A patch is an immutable unit of code change. We can create them, version them, and apply them to one or many branches.

Unlike commits, patches are not tied to branches, so multiple patches can be applied to the same branch at the same time. For example, you can apply a set of patches to your branch while doing other work on the same branch. This is much more flexible than fetching and merging pull request branches, which are often not versioned, contain too many fixup commits, and are hard to review.

Gitpatch allows each patch version to be reviewed independently, so you can get feedback early on in-progress code or push multiple alternative versions of the same feature.

Patches can be stacked on top of each other, which allows large features to be split into smaller chunks. Each patch can then be reviewed independently, speeding up the overall review process.

Each patch maps to a single clean commit, so it's easy to see which patches are applied in the git log.

Traditionally, patches are shared over email, but they can be shared over other mediums. For example, we can generate them from commits pushed over existing Git SSH and HTTPS protocols.
That's the idea behind Gitpatch, a patch-based workflow that doesn't require email.

## Submitting a Patch

To submit a patch, create a new branch that starts with `patch/`, make your changes, and push it. That's it! Gitpatch will automatically turn that into a patch with a unique link. All commits on this branch are squashed into a single patch.

Example:

```bash
git clone <repository>
git switch -c patch/my-fix
# make and add changes
git commit
git push
```

If your commit message includes a longer description, it will be rendered using markdown. When patch is applied, the full description will be preserved in the commit log.

When you push a patch branch, Gitpatch assigns branch ownership to you. After that, only you (and repository maintainers) can update that branch.

## Updating a Patch

To update a patch, push more commits to the same branch. You can add commits, amend them, or rewrite history; Gitpatch will keep track of all the versions. Reviewers can view and compare previous versions of the patch as it evolves.

## Applying a Patch

Gitpatch generates a `*.patch` URL for each patch and a one-liner git command to apply it locally. This uses `git am` command to apply it.

## Working with Patch Stacks

If your work involves a series of related changes, you can submit them as a patch stack. To do that, create a branch that starts with `patchstack/` and make multiple commits. Each commit will be become a separate patch in the stack.

Example:

```bash
git clone <repository>
git switch -c patchstack/my-feature
git commit -m 'Part 1: Add helpers'
git commit -m 'Part 2: Use helpers in logic'
git push
```

To update the stack, you can reorder, squash, or amend commits and then force-push. Gitpatch will match the updated commits to the original ones and will update patches as necessary.

You can also use git rebase's `--fixup` flag to quickly tweak earlier commits in your stack. For convenience, Gitpatch provides a command to do this.

Patches in the patch stack can also be added or removed by adding or removing commits.

## Patch Versioning

Gitpatch saves every version of your patch, even if you force-push. It compares the actual content rather than commit hashes, so changing just the hash won't create a new version.

## Tools for Maintainers

If you're reviewing incoming patches, Gitpatch provides a few useful tools:

* You can add comments to the whole patch or individual lines.
* You can quickly archive patches that don't meet your project's standard.
* You can apply patches locally with one command.
* You can accept patches once they're reviewed or use "Accept with Changes" if you're okay with the author making final tweaks.

_Note_: Gitpatch doesn't apply or merge anything automatically yet. If a patch is accepted, it still needs to be applied manually and pushed to the main branch.

## Storage

Every push is stored on at least two Git servers and backed up to an S3-like object storage for durability.

## Where Gitpatch Comes From

Gitpatch was inspired by the workflows used in large open source projects like the Linux kernel and Git itself. These projects primarily use email and patches rather than pull requests.

It also gets ideas from `git-send-email`, tools for managing stacks, and [this gist](https://gist.github.com/thoughtpolice/9c45287550a56b2047c6311fbadebed2).

This is not a new approach. Other tools exist that offer similar patch-based workflows, and integrate with GitHub or GitLab. Gitpatch takes a bit different route, making patches and stacks a first-class feature on top of git. If you're curious what else is out there, see [alternatives here](alternatives.md).

---

## Try It Out

Gitpatch is currently in beta, so things may not be stable. We're interested in your feedback. Email us at **hi@gitpatch.com**.

[Sign up here](https://gitpatch.com/signup) or see example [git.git](https://gitpatch.com/gitpatch/git-demo) repository demo.

## Links

* [Get Started](./get-started.md)
