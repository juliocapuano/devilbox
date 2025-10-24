# Custom Startup Scripts (Global)

## Overview

Any script in this directory with a `.sh` extension will be automatically executed during PHP container startup. This is particularly useful for applying custom settings or installing software that typically requires user interaction, such as accepting licenses.

## Available Example Scripts

### 1. Blackfire CLI Configuration (`configure-blackfire-cli.sh-example`)
Configures the Blackfire CLI tool with your credentials. To use:
1. Copy to `configure-blackfire-cli.sh`
2. Set the following environment variables in your `.env` file:
   ```
   BLACKFIRE_CLIENT_ID=your-client-id
   BLACKFIRE_CLIENT_TOKEN=your-client-token
   ```

### 2. Composer Version Management (`downgrade-composer-twopoint2.sh-example`)
Downgrades Composer to version 2.2. To use:
1. Copy to `downgrade-composer-twopoint2.sh`
2. The script will automatically run during container startup

### 3. Permission Fixes
- `fix-ownership-logs.sh-example`: Fixes log directory ownership
- `fix-ownership-nvm.sh-example`: Fixes NVM directory ownership

### 4. Node.js Project Autostart (`run-node-js-projects.sh-example`)
Automatically starts Node.js applications using PM2. To use:
1. Copy to `run-node-js-projects.sh`
2. Customize the script to point to your Node.js applications

## Usage

1. Copy any of the provided `*.sh-example` files to a new file with a `.sh` extension
2. Customize the script as needed
3. The script will automatically run on container startup

## Running Commands as Non-Root User

By default, scripts run as root. To run commands as the `devilbox` user, use:

```bash
su -c 'your-command-here' -l devilbox
```

For Node.js applications using PM2:

```bash
su -c 'cd /path/to/your/app && pm2 start index.js' -l devilbox
```

## PHP Version-Specific Scripts

This directory runs commands for all PHP versions. For version-specific configurations, place your scripts in the appropriate directory:

- `cfg/php-startup-7.4/` for PHP 7.4
- `cfg/php-startup-8.0/` for PHP 8.0
- (and so on for other versions)

## Important Notes

- **Root Permissions**: All scripts in this directory run with **root** privileges
- **File Permissions**: Ensure your scripts are executable (`chmod +x script.sh`)
- **Debugging**: Add `set -x` at the beginning of your script for debugging
- **Error Handling**: Use `set -euo pipefail` at the start of your scripts for better error handling
- **Logs**: Check container logs if your scripts aren't running as expected

## Best Practices

1. Always test scripts in a non-production environment first
2. Add error handling to your scripts
3. Include comments explaining what each script does
4. Keep scripts idempotent (safe to run multiple times)
5. Use environment variables for sensitive information
