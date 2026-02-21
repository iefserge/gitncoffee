# Philosophy

Great performance is not just a nice thing, fast software can be used differently and more creatively.

## Development

The service has been in development since April 2023, publicly available since 2025.
It has been renamed from Gitpatch to Git ‘n’ Coffee in January 2026.

- Git ‘n’ Coffee builds its own backend and storage system for Git from scratch that is designed for its purpose.
- In the browser, Git ‘n’ Coffee uses as little client-side code as possible to ensure that the UI is immediately responsive.
- Git ‘n’ Coffee itself is hosted on Git ‘n’ Coffee from day one

## Storage

Git ‘n’ Coffee implements an advanced Git storage backend system that supports replication and backups.
Every push is stored on at least two Git servers and backed up to an S3-like object storage for durability.
