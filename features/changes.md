# Changes

<img src="../image/changes.png" width="600" title="Changes">

A change is a commit that has a stable identifier. You can amend or rewrite commit, but its Change ID stays the same.
Each change gets its own code review page and can be approved independently.

Multiple changes can be stacked or top of each other, which makes stacked diffs possible. Multiple
people can work on different changes in the same stack, without risking overwriting each other's work.

## Change IDs and Git

Git currently doesn't have a concept of Change IDs. To support Git, Git ‘n’ Coffee generates Change IDs
automatically.

There are two ways they are generated:
- By hashing commit author name, email and timestamp. This works with no extra configuration.
    Rebases or amends don't update those, so it's safe to use as a stable identifier in most cases.
    Hash function is defined as `SHA1("coffee {name}:{email}:{timestamp}")`.
- By adding `Change-ID:` trailer to the commit message. It's used as is, if it matches Jujutsu
    Change ID format. Otherwise, the value from the header is hashed as `SHA1("coffee {change_id_trailer}")`

In both cases, first 16 bytes of the SHA1 hash are converted to
Jujutsu Change ID format (reverse hex with `z-k` letters which map to `0-f`).

## Change IDs and Jujutsu

Jujutsu includes Change IDs in commit headers by default.

## Code Review Branches

Git ‘n’ Coffee supports special named branches:

- `change/*`
- `push-*` and prefixed `*/push-*` (Jujutsu-style branches)

They all work the same. All changes pushed to those branches are submitted for review.
Excluding maintainers, only the branch creator can push to it.

The same change can be updated using any branch, not necessarily the one that was used to create it.

## Collaboration

Multiple people can work on different parts of the change stack and can branch out from any point in the stack.

By default, only the change submitter can update their change. They can amend their
changes in the stack without overwriting other work in the same stack.

## Amending changes

Rewrite history and force push commits to update them. Amend or rebase is always preferred when working with changes.
Amending commits helps to keep change stacks smaller and easier to apply later.

## Closing changes

Changes are closed when there are no branches that contain them. You can delete any unneeded branches to abandon those
changes.

## Approvals

Changes can be acknowledged (ack) or approved. Precise meaning of acknowledgement is not defined and can be different
for each workflow and team.

## Applying changes

_This is work in progress._

Changes can be applied (i.e. merged) in their order in the stack. Rebase can be used to reorder changes.

Out-of-order apply may be possible in the future, but will likely require an additional CI run.
