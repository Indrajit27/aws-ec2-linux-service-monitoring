# AWS EC2 Linux Service Monitoring

A hands-on lab for managing and monitoring services on an Amazon Linux 2 EC2 instance.

## Lab objectives

- Check the status of the Apache HTTP server (`httpd`).
- Start, stop, restart, and enable the service with `systemctl`.
- Verify that the web server responds over HTTP.
- Monitor Linux processes and resource usage with `top`.
- Observe EC2 performance metrics in Amazon CloudWatch.

## Architecture

```text
Client browser
     |
     | HTTP (TCP 80)
     v
Amazon Linux 2 EC2 instance
     |
     +-- Apache httpd service
     +-- Linux process monitor (top)
     |
     v
Amazon CloudWatch EC2 metrics
```

## Prerequisites

- An AWS account with permission to create and view EC2 instances and CloudWatch metrics.
- An Amazon Linux 2 EC2 instance.
- SSH access to the instance.
- A security group allowing SSH (TCP 22) from your trusted IP and HTTP (TCP 80) from the clients you intend to use.

> **Security:** Do not expose SSH to the entire internet. Do not commit private keys, AWS credentials, account IDs, or sensitive public IP addresses to this repository.

## Lab procedure

### 1. Connect to the EC2 instance

Connect using SSH or a terminal client such as PuTTY. After connecting, confirm the operating system:

```bash
cat /etc/os-release
```

### 2. Check Apache service status

```bash
sudo systemctl status httpd
```

If Apache is not installed, install it first:

```bash
sudo yum install -y httpd
```

Start the service and configure it to start automatically after a reboot:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Useful service-management commands:

```bash
sudo systemctl status httpd
sudo systemctl stop httpd
sudo systemctl restart httpd
sudo systemctl is-enabled httpd
```

### 3. Test the HTTP service

Create a simple test page:

```bash
echo "Apache is running on my EC2 instance" | sudo tee /var/www/html/index.html
```

On the EC2 instance, test the local web server:

```bash
curl -I http://localhost
curl http://localhost
```

Then open `http://<EC2-PUBLIC-IP>` in a browser. Replace the placeholder with the instance's public IPv4 address. Confirm that the security group permits inbound TCP port 80.

### 4. Monitor Linux processes

Run:

```bash
top
```

In `top`, review CPU utilization, memory and swap usage, load averages, process IDs, and process states. Press `q` to exit.

### 5. Review CloudWatch metrics

In the AWS Console, open **CloudWatch → Metrics → EC2 → Per-Instance Metrics** and select your instance.

Review the available metrics, including:

- `CPUUtilization`
- `DiskReadBytes` and `DiskReadOps`
- `DiskWriteBytes` and `DiskWriteOps`
- `NetworkIn` and `NetworkOut`

Metric availability and reporting intervals can vary. Basic EC2 monitoring commonly reports metrics at five-minute intervals; detailed monitoring provides one-minute intervals and may incur additional charges.

## Troubleshooting

| Symptom | Checks |
|---|---|
| `httpd` is inactive | Run `sudo systemctl status httpd` and inspect `sudo journalctl -u httpd`. |
| HTTP request fails locally | Check service status and inspect Apache logs under `/var/log/httpd/`. |
| Browser cannot reach the instance | Verify the public IP, instance network path, and inbound TCP 80 security-group rule. |
| SSH connection fails | Verify the instance is running, the correct key is used, and inbound TCP 22 is restricted to your IP. |
| CloudWatch metrics are not visible | Select the correct region and instance; allow time for metrics to publish. |

## Repository contents

- `README.md` — lab overview, procedure, and troubleshooting.
- `docs/monitoring-commands.md` — quick reference for Linux and service-monitoring commands.

## Evidence and learning notes

Add screenshots captured from your own lab in a `screenshots/` folder. Useful evidence includes the `systemctl status httpd` output, successful HTTP response, `top` output, and the EC2 CloudWatch metrics graph.

Only describe results you personally observed. This repository documents the procedure; it does not claim that a particular test result or performance improvement was measured.

## Cleanup

Stop or terminate test resources when finished to avoid unnecessary AWS charges. Review any attached storage, snapshots, Elastic IPs, or other billable resources before cleanup.

## Disclaimer

This is a learning lab. Commands and console labels may vary with the operating-system image and AWS console updates. Review AWS costs and security settings before running workloads.
