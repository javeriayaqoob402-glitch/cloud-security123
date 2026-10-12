# EC2 Hardening Notes — SSH & Nginx
Easy English / Roman Urdu | GitHub-ready

## 1. SSH Hardening

SSH server ko secure karne ke liye.

### Commands

1. Edit SSH configuration:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Test the SSH configuration:
   ```bash
   sudo sshd -t
   ```
   If the command returns no output, the configuration test usually passed.

3. Reload SSH after a successful test:
   ```bash
   sudo systemctl reload ssh
   ```
   On Amazon Linux, the service is commonly named `sshd`:
   ```bash
   sudo systemctl reload sshd
   ```

4. Check service status:
   ```bash
   sudo systemctl status ssh
   ```
   On Amazon Linux, use:
   ```bash
   sudo systemctl status sshd
   ```

### Example SSH security settings

Edit `/etc/ssh/sshd_config` and review these settings:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

**Warning:** Set `PasswordAuthentication no` only after confirming SSH-key login works. Keep your current SSH session open and test a second login before closing it. Otherwise, you may lock yourself out of the EC2 instance. Also confirm your AMI's defaults and requirements before changing settings.

## 2. Nginx Hardening

Nginx is a web server. Its configuration and commands are different from SSH.

### Commands

1. Edit the main Nginx configuration:
   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

2. Test the Nginx configuration:
   ```bash
   sudo nginx -t
   ```

3. Reload Nginx only if the test succeeds:
   ```bash
   sudo systemctl reload nginx
   ```

4. Check service status:
   ```bash
   sudo systemctl status nginx
   ```

### Example Nginx security setting

Inside the appropriate `http` or `server` block, you can use:

```nginx
server_tokens off;
```

This helps hide the Nginx version in error pages and certain response headers. It is not a complete security solution.

## 3. SSH vs Nginx — Quick Comparison

| Task | SSH | Nginx |
|---|---|---|
| Configuration file | `/etc/ssh/sshd_config` | `/etc/nginx/nginx.conf` |
| Edit | `sudo nano /etc/ssh/sshd_config` | `sudo nano /etc/nginx/nginx.conf` |
| Test | `sudo sshd -t` | `sudo nginx -t` |
| Reload (Ubuntu, commonly) | `sudo systemctl reload ssh` | `sudo systemctl reload nginx` |
| Reload (Amazon Linux, commonly) | `sudo systemctl reload sshd` | `sudo systemctl reload nginx` |
| Check status (Ubuntu, commonly) | `sudo systemctl status ssh` | `sudo systemctl status nginx` |
| Check status (Amazon Linux, commonly) | `sudo systemctl status sshd` | `sudo systemctl status nginx` |

**Note:** Service names can vary by Linux distribution and image. If a command says the unit cannot be found, check the service name on your instance.

## 4. Safe Workflow

Remember:

**Edit → Test → Reload → Verify**

- Edit the configuration.
- Test it before applying changes.
- Reload only if the test succeeds.
- Verify service status and connectivity.

### EC2 lockout prevention checklist

- Keep your current SSH session open while making changes.
- Confirm your key-based login works before disabling password authentication.
- Make sure the EC2 Security Group allows SSH (TCP port 22) only from trusted IP addresses where practical.
- Test a second SSH session before closing the original one.
- Do not change several access settings at once.
- If remote access fails, use your approved recovery method for the instance.

## 5. Practice Task

1. Check whether SSH and Nginx are installed/running.
2. Review each configuration file without changing anything.
3. Run `sudo sshd -t` for SSH and `sudo nginx -t` for Nginx.
4. Record the results in your lab notes.
5. Only make changes in a test environment and verify access afterwards.
