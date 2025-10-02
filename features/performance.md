# Fast Software

One of the values of Gitpatch is that we believe that fast software can change how we work and can make more things possible. This means great performance is not just a nice thing, but it is something that changes behavior and unlocks new possibilities. Fast software can be used differently and more creatively.

On the other hand, slow software doesn't respect our time. Slow software is complicated or does too much unnecessary work. Slow software makes us tired.

This belief influences how Gitpatch makes technical decisions:
- Gitpatch builds its own backend and storage system for Git from scratch that is specifically designed for this purpose.
- In the browser, Gitpatch uses as little client-side code as possible to ensure that the UI is immediately responsive.
- Gitpatch is built on Gitpatch from day one, so every change affects us as well

The goal, if Gitpatch is not the fastest Git service out there, then it's bug for us to fix.

Related articles about fast software:

- https://www.catherinejue.com/fast
- https://bdickason.com/posts/speed-is-the-killer-feature/

## Development

The service has been in development since April 2023, publicly available since 2025.

## Storage

Every push is stored on at least two Git servers and backed up to an S3-like object storage for durability.
