# SOP for Cron jobs 

|   Authors        |  Created on   |  Version   | Last updated by | Last edited on |
| -----------------| --------------| -----------|---------------- | -------------- |
| Kawalpreet Kour      | 16 July 2025   |     v1     |    -     |   -    |

# SOP for Cron Jobs in Ubuntu

## Table of Contents
- [Introduction](#introduction)
- [Purpose](#purpose)
- [Cron Installation and Service Check](#cron-installation-and-service-check)
- [Cron Job Syntax (Timing Format)](#cron-job-syntax-timing-format)
- [Create a Cron Job](#create-a-cron-job)
- [Edit or Remove Cron Jobs](#edit-or-remove-cron-jobs)
- [Check Cron Logs (Job Output Verification)](#check-cron-logs-job-output-verification)
- [Restart Cron Service (if needed)](#restart-cron-service-if-needed)

## Introduction
Cron is a time-based job scheduler in Unix-like systems (including Ubuntu). It allows users to automate commands or scripts to run at specific intervals – like daily, weekly, or on reboot – without manual intervention.

## Purpose
The main purpose of cron is automation – to save time and reduce errors by running repetitive tasks automatically. Common use cases include:

- Running scripts at fixed time (e.g., every night at 2 AM)
- Daily backup of files or databases
- Monitoring services or cleaning temporary files
- Sending automatic reports

## Cron Installation and Service Check

Check if cron is installed:
```bash
dpkg -l | grep cron
```

Install (if not installed):
```bash
sudo apt update
sudo apt install cron
```

Enable and start the service:
```bash
sudo systemctl enable cron
sudo systemctl start cron
```

Check service status:
```bash
sudo systemctl status cron
```

## Cron Job Syntax (Timing Format)
Each cron job line has 5 time fields followed by the command to execute:

```
*  *  *  *  *  command-to-run
|  |  |  |  |
|  |  |  |  └─ Day of the Week (0 - 7) (Sunday = 0 or 7)
|  |  |  └──── Month (1 - 12)
|  |  └─────── Day of Month (1 - 31)
|  └────────── Hour (0 - 23)
└───────────── Minute (0 - 59)
```

**Examples:**

| Schedule              | Cron Syntax     | Meaning                   |
|-----------------------|------------------|-----------------------------|
| Every minute          | `* * * * *`      | Runs every minute           |
| Every day at 2 AM     | `0 2 * * *`      | Runs daily at 2:00 AM       |
| Every Monday at 5 PM  | `0 17 * * 1`     | Runs every Monday at 5 PM   |
| Every 15 minutes      | `*/15 * * * *`   | Runs every 15 minutes       |

## Create a Cron Job

For current user:
```bash
crontab -e
```
Add a new line like:
```bash
0 10 * * * /home/kawal/scripts/backup.sh
```
This runs `backup.sh` every day at 10 AM.

For root/system-wide:
```bash
sudo nano /etc/crontab
```

## Edit or Remove Cron Jobs

View current user's cron jobs:
```bash
crontab -l
```

Edit:
```bash
crontab -e
```

Remove all jobs:
```bash
crontab -r
```

## Check Cron Logs (Job Output Verification)

Check logs to see if cron jobs ran:
```bash
grep CRON /var/log/syslog
```
Or use journalctl:
```bash
sudo journalctl -u cron
```
Redirect cron output to a file:
```bash
0 10 * * * /home/kawal/backup.sh >> /home/kawal/logs/backup.log 2>&1
```

## Restart Cron Service (if needed)

If changes are not reflecting or cron is stuck:
```bash
sudo systemctl restart cron
```  
***
# Contact Information

|Kawalpreet Kour                     | Kawalpreet.kour.snaatak@mygurukulam.co                                                                                      
|---------------------------------|------------------------------------------------------------|

***
# References

| Links | Descriptions |
| :---- | :----------- |
| https://youtu.be/YA1wPJxeAmg?si=_oMBegTpPa3UfpIw | Document format followed from this link |
| https://cronitor.io/guides/cron-jobs#what-is-a-cron-job | This link explains Cron basics and syntax  |
| https://crontab.guru/ | The quick and simple editor for cron schedule expressions by Cronitor.|

