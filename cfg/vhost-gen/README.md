# vHost Generator Configuration

## Overview

The vhost-gen system allows you to customize web server configurations for your projects in the Devilbox environment. These configurations are automatically generated based on templates that you can customize to fit your specific needs.

## Template Types

### 1. Global vs Project-Specific Templates

- **Global Templates**: Placed in this directory (`/cfg/vhost-gen/`), these templates apply to all projects that don't have their own specific configuration.
- **Project-Specific Templates**: Placed in a project's root directory, these override global settings for that specific project.

### 2. Template Categories

#### Standard Virtual Hosts

These templates are used for regular web applications:

| Web Server     | Example Template               | Template Filename | Description |
|----------------|--------------------------------|-------------------|-------------|
| Apache 2.2     | `apache22.yml-example-vhost`   | `apache22.yml`    | For Apache 2.2 server configurations |
| Apache 2.4     | `apache24.yml-example-vhost`   | `apache24.yml`    | For Apache 2.4 server configurations |
| Nginx          | `nginx.yml-example-vhost`      | `nginx.yml`       | For both Nginx stable and mainline |

#### Reverse Proxy Virtual Hosts

**⚠️ Important**: These templates should only be used for specific projects, not globally.

| Web Server     | Example Template               | Template Filename | Description |
|----------------|--------------------------------|-------------------|-------------|
| Apache 2.2     | `apache22.yml-example-rproxy`  | `apache22.yml`    | Apache 2.2 reverse proxy configuration |
| Apache 2.4     | `apache24.yml-example-rproxy`  | `apache24.yml`    | Apache 2.4 reverse proxy configuration |
| Nginx          | `nginx.yml-example-rproxy`     | `nginx.yml`       | Nginx reverse proxy configuration |

#### Specialized Templates

- **Magento 2**: `nginx.yml-example-magento2` - Optimized configuration for Magento 2 stores
- **PHP Multi-version**: `backend.cfg-example-php-multi` - For running multiple PHP versions
- **Reverse Proxy Multi**: `backend.cfg-example-rproxy-multi` - For multiple reverse proxy configurations

## How to Use

1. **Enable a Template**:
   ```bash
   # For global usage
   cp nginx.yml-example-vhost nginx.yml
   
   # For project-specific usage (in your project directory)
   cp /path/to/devilbox/cfg/vhost-gen/nginx.yml-example-vhost /shared/httpd/your-project/nginx.yml
   ```

2. **Customize the Template**:
   - Edit the template file to match your requirements
   - Common customizations include:
     - Custom error pages
     - Security headers
     - CORS settings
     - Cache control
     - SSL/TLS configurations

3. **Apply Changes**:
   - Restart the web server container to apply changes:
     ```bash
     docker-compose restart httpd
     # or
     docker-compose restart nginx
     ```

## Template Variables

Templates use variables that are automatically replaced during vhost generation:

| Variable | Description |
|----------|-------------|
| `__VHOST_NAME__` | The virtual host name |
| `__DOCUMENT_ROOT__` | Path to the document root |
| `__PHP_ADDR__` | PHP-FPM service address |
| `__PHP_PORT__` | PHP-FPM service port |
| `__PORT__` | HTTP/HTTPS port |
| `__HTTP_PROTO__` | HTTP protocol (http/2 support) |
| `__DEFAULT_VHOST__` | Default vhost configuration |

## Best Practices

1. **Backup First**: Always back up existing configurations before making changes.
2. **Test Locally**: Test configurations in a development environment first.
3. **Use Project-Specific**: Prefer project-specific configurations over global ones.
4. **Document Changes**: Add comments in your custom templates for future reference.
5. **Version Control**: Keep your custom templates under version control.

## Troubleshooting

- **Configuration Not Applied**: Ensure the template has the correct filename and is in the right location.
- **Permission Issues**: Check file permissions if the web server can't read the configuration.
- **Syntax Errors**: Use server-specific tools to check configuration syntax:
  ```bash
  # For Nginx
  nginx -t
  
  # For Apache
  apachectl -t
  ```

## Example: Enabling PHP-FPM

To enable PHP-FPM processing in your vhost, ensure these lines are present in your template:

```nginx
location ~ \.php$ {
    fastcgi_pass   __PHP_ADDR__:__PHP_PORT__;
    fastcgi_index  index.php;
    include        fastcgi_params;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
}
```

## Support

For more information, refer to:
- [Devilbox Documentation](https://devilbox.readthedocs.io/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Apache Documentation](https://httpd.apache.org/docs/)
