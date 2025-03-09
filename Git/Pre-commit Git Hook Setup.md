###  **Direct Git Hook Setup (No `.pre-commit-config.yaml`)**

The solution you’re working on is based on **Git's native hooks** rather than the pre-commit framework. The **`prepare-commit-msg`** hook is part of Git itself and does not require the `.pre-commit-config.yaml` file. You can directly use it to modify commit messages.
### Steps:

1. **Create/Modify the `prepare-commit-msg` Hook**: Go to your repository's `.git/hooks/` directory and create or modify the `prepare-commit-msg` file to append the branch name to the commit message.

- Navigate to `.git/hooks/`:

```bash
cd .git/hooks
```

Create or edit the `prepare-commit-msg` file:

```bash
touch prepare-commit-msg 
chmod +x prepare-commit-msg
```

Open the `prepare-commit-msg` file and paste this content:

```bash
#!/bin/sh

# Get the current branch name
BRANCH_NAME=$(git rev-parse --abbrev-ref HEAD)

# Get the commit message file path passed by Git
COMMIT_MSG_FILE=$1

# Check if the commit message is a merge commit (avoid appending branch to merge commit messages)
if [ -z "$(grep '^Merge' "$COMMIT_MSG_FILE")" ]; then
    # Append the branch name to the commit message
    echo "\n\n[Branch: $BRANCH_NAME]" >> "$COMMIT_MSG_FILE"
fi
```

1. **Test the Hook**: After setting up the `prepare-commit-msg` hook, you can test it by making a commit in your repository.

```bash
git add .
git commit -m "Fix bug in feature X"
```

When you check the commit message (before you finalize the commit), the branch name should be appended to it, like:

```csharp
Fix bug in feature X

[Branch: my-feature-branch]
```

