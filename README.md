# Automatic WordPress on LAMP (BASH)

A small interactive Bash installer that attempts to install and configure a LAMP stack and deploy WordPress automatically.

Files

- [wordpress_lamp.sh](wordpress_lamp.sh) — main installer script. Key functions:
  - [`lamp_install`](wordpress_lamp.sh)
  - [`apache_virtual_host_setup`](wordpress_lamp.sh)
  - [`ssl_config`](wordpress_lamp.sh)
  - [`db_config`](wordpress_lamp.sh)
  - [`wordpress_config`](wordpress_lamp.sh)
  - [`execute`](wordpress_lamp.sh)
- [LICENSE](LICENSE) — project license.

Requirements

- Debian/Ubuntu-based system.
- Root (sudo) access.
- Internet access to download packages and WordPress.

Usage

1. Make the script executable:
   ```sh
   sudo chmod +x wordpress_lamp.sh
   ```
2. Run the script with root privileges:
   ```sh
   sudo bash wordpress_lamp.sh
   ```
3. The script will prompt for:
   - Domain (without www)
   - Database username
   - Database password
   - Database name

What the script does (high level)

- [`lamp_install`](wordpress_lamp.sh): updates apt, installs UFW, Apache, MariaDB and PHP, enables required Apache modules.
- [`apache_virtual_host_setup`](wordpress_lamp.sh): creates a site directory, writes an Apache vhost file, enables the site.
- [`ssl_config`](wordpress_lamp.sh): creates a self-signed certificate and writes SSL parameters and enables SSL site/config.
- [`db_config`](wordpress_lamp.sh): creates the MySQL/MariaDB database and grants privileges for the provided DB user.
- [`wordpress_config`](wordpress_lamp.sh): downloads WordPress, copies files to the site directory, adjusts permissions, and updates wp-config.php.
- [`execute`](wordpress_lamp.sh): runs the above steps in order.

Important notes / Known issues

- The script contains multiple syntax and path typos that will prevent successful execution as-is (for example paths like `/etc/apache2/sitesavailable` vs `/etc/apache2/sites-available`, `/etc/apache2/modsenabled` vs `/etc/apache2/mods-enabled`, broken multi-line commands and sed invocations). Review and fix these before running.
- The shebang is `#!/bin/shell` — consider using `#!/bin/bash` for better compatibility.
- Some package commands do not use `-y` or call interactive tools (e.g., `mysql_secure_installation`) — these will pause for input.
- The script creates a self-signed SSL certificate — for public sites use a trusted CA (Let's Encrypt).
- Running this on a production server without review is not recommended. Test in a VM or disposable environment first.

Security and permissions

- The script runs system-wide operations and must be run as root.
- Store database credentials securely. The script writes credentials directly into wp-config.php; consider safer secret handling.

Contributing / Improvements

- Fix path typos and broken sed/openssl lines.
- Add argument support (non-interactive mode).
- Add checks for each step and rollback on failure.
- Use Certbot to obtain real SSL certificates.

License
This repository is distributed under the terms of the Apache License 2.0.
