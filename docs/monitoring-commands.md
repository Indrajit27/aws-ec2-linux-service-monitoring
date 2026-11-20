# Linux and EC2 Monitoring Command Reference

## Service management (systemd)

```bash
# Check service status
sudo systemctl status httpd

# Start, stop, and restart Apache
sudo systemctl start httpd
sudo systemctl stop httpd
sudo systemctl restart httpd

# Enable service at boot and verify
sudo systemctl enable httpd
sudo systemctl is-enabled httpd

# View recent service logs
sudo journalctl -u httpd --since "30 minutes ago"
```

## HTTP checks

```bash
# Check local HTTP response headers
curl -I http://localhost

# Fetch the local page
curl http://localhost

# Check listening TCP sockets
sudo ss -lntp
```

## Linux resource monitoring

```bash
# Interactive process and resource monitor
top

# Memory summary
free -h

# Filesystem capacity
df -h

# Disk usage by block device
lsblk

# System uptime and load averages
uptime

# CPU information
lscpu
```

## Apache logs

```bash
# Recent access log entries
sudo tail -n 50 /var/log/httpd/access_log

# Recent error log entries
sudo tail -n 50 /var/log/httpd/error_log
```

## CloudWatch checklist

In **CloudWatch → Metrics → EC2 → Per-Instance Metrics**, select the correct instance and review:

- CPUUtilization
- DiskReadBytes / DiskReadOps
- DiskWriteBytes / DiskWriteOps
- NetworkIn / NetworkOut

EC2 basic monitoring commonly publishes at five-minute intervals. Detailed monitoring provides one-minute intervals and may incur additional charges. Not every metric is available for every instance or configuration.

## Operational reminders

- Use least-privilege IAM permissions.
- Restrict SSH ingress to a trusted source IP.
- Do not put credentials, private keys, or sensitive infrastructure details in Git.
- Capture real lab output for portfolio evidence; do not fabricate screenshots or results.
