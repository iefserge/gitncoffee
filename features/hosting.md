# Repository Hosting

*At the moment, only public repositories are supported. Private repositories and organizations are still in development.*

Git ‘n’ Coffee currently supports Git SSH protocol only. You can create a repository by doing the initial push. For example:

```sh
git init
git remote add origin git@gitncoffee.com:<username>/<repository>
echo "# Hello, World" > README.md
git add -A && git commit -m "First commit"
git push
```

- Replace `<username>` with your Git ‘n’ Coffee username
- Replace `<repository>` with any name for your repository

## Mirroring

When you clone a Git repository, Git creates a default `origin` remote, which is the destination
from which the repository was cloned.

However, you can also add additional remotes to the same repository, to be able to `fetch` and `push` it
to additional destinations. This is useful for mirroring the same repository between multiple remote hosts.

For example, if you have a repository that is hosted elsewhere, you can mirror it to Git ‘n’ Coffee:

```bash
git remote add gitncoffee git@gitncoffee.com:<username>/<repository>
git push gitncoffee --mirror
```

Read more about Git remotes:

- https://git-scm.com/book/ms/v2/Git-Basics-Working-with-Remotes

## Limits

Git ‘n’ Coffee applies the following limits to repositories:
- Maximum file size is 100MB.
- Maximum push size is 2GB.
- Maximum push request duration is 15 minutes.

For patches from non-committers, push size is limited to 100MB.

Please contact support@gitncoffee.com if you have issues or are hitting the limits.
