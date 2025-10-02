# Generate an SSH key

If you don't already have an SSH key, you can create one with:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Follow the prompts to save the key (usually in `~/.ssh/id_ed25519`). Then, make sure your SSH agent is running and add the key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Read more:

- https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key.