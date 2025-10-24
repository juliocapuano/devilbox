# Bash Configuration Directory

This directory contains custom bash configuration files that are automatically loaded by the Devilbox environment.

## Overview

Files in this directory serve the following purposes:

1. They are automatically mounted to `/etc/bashrc-devilbox.d/` in the container
2. Any file with a `.sh` extension will be automatically sourced by bash
3. These configurations are loaded for both the `devilbox` and `root` users

## Usage

### Adding Custom Configurations

1. Create a new `.sh` file in this directory
2. Add your custom bash configurations, aliases, or functions
3. The file will be automatically sourced when a new shell is started

### Example: Custom Vim Configuration

You can create a custom vim configuration by:

1. Creating a `vimrc` file in this directory
2. Adding your vim configurations
3. Using the provided alias to load it automatically:

```bash
# This alias is already set up in bashrc.sh-example
alias vim='vim -u /etc/bashrc-devilbox.d/vimrc'
```

## Best Practices

- Use descriptive filenames (e.g., `aliases.sh`, `functions.sh`)
- Keep related configurations together in the same file
- Add comments to explain complex configurations
- Test your changes in a new shell session

## Troubleshooting

- If a configuration isn't loading, check the file has a `.sh` extension
- Verify file permissions are set to be readable
- Check for syntax errors in your scripts

## Note

Changes to these files require a new shell session to take effect. Use `exec bash` to reload your shell configuration in the current session.