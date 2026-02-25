<img src="image/logo.png" width="120" title="Git ‘n’ Coffee">

# Welcome to Git ‘n’ Coffee!

_Caffeinated Jujutsu-flavored take on Git._

Git ‘n’ Coffee is a source code hosting and code review service built on top of Git repository format
and network protocol. It supports any version control system that can talk Git protocol.

Git ‘n’ Coffee offers a commit-based collaboration and code review workflow using Jujutsu-style Change IDs.
This makes it possible to share and review entire stacks of changes. Multiple people can
collaborate on the same stack, without overwriting each other's work.

Currently, only cloud-based version is available. In the future, self-hostable version is planned to be released.

Git ‘n’ Coffee is built from scratch with heavy focus on performance.

## Features

- **Repository Hosting:** Mirror any repository to Git ‘n’ Coffee by pushing to the remote URL.
    Repository will be automatically created on the first push.
- **Code Review:** Submit changes for review by pushing named branches that
    begin with `change/*` or `push-*` (Jujutsu-style push branches). These branches are processed differently from
    regular long lived branches.
- **Fast and Beautiful UI:** An unusually fast web interface to browse files, commit history and to review changes.

## Read more

* [More about changes](./features/changes.md)
* [Read about Git hosting](./features/hosting.md)
* [Read about AI policy](./features/ai.md)
* [Change Log](./CHANGELOG.md)

## Guides

- [Get Started](./guides/get-started.md)
- [Generate SSH key](./guides/generate-ssh.md)

## Try It Out

Join [Discord](https://discord.gg/9qr6cUm4bu).

Git ‘n’ Coffee is currently in beta, and many important features are still missing.
Send your feedback to hi@gitncoffee.com.

You can [sign up here](https://gitncoffee.com/signup) or see example
[git.git](https://gitncoffee.com/gitncoffee/git-demo) repository.
