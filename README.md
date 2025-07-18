# SOP for Cron Jobs in Ubuntu

| Authors           | Created on     | Version | Last updated by | Last edited on |
|-------------------|---------------|---------|-----------------|---------------|
| Kawalpreet Kour   | 16 July 2025  | v1      | -               | -             |

---

## Table of Contents

- [Introduction](#introduction)
- [Purpose](#purpose)
- [Cron Installation and Service Check](#cron-installation-and-service-check)
- [Cron Job Syntax and Examples](#cron-job-syntax-and-examples)
- [How to Create a Cron Job](#how-to-create-a-cron-job)
- [How to Edit or Remove Cron Jobs](#how-to-edit-or-remove-cron-jobs)
- [How to Check Cron Logs](#how-to-check-cron-logs)
- [How to Restart Cron Service](#how-to-restart-cron-service)
- [Best Practices & Troubleshooting](#best-practices--troubleshooting)
- [Contact Information](#contact-information)
- [References](#references)

---

## Introduction

Cron is a time-based job scheduler in Unix-like systems (including Ubuntu). It enables users to automate commands or scripts at specific times and intervals (daily, weekly, on reboot, etc.) with no manual intervention.

---

## Purpose

Automating repetitive tasks with cron saves time and reduces errors. Typical use cases include:
- Running scripts at fixed times (e.g., every night at 2 AM)
- Backing up files or databases
- Cleaning up temporary files
- Monitoring services or generating reports

---

## Cron Installation and Service Check

1. **Check if cron is installed:**
    ```bash
    dpkg -l | grep cron
    ```
    ![Checking if cron is installed: This screenshot shows the output of checking for the cron package using dpkg. If 'cron' appears in the list, it means cron is installed on your system.](https://github.com/user-attachments/assets/95a1ddd1-3c21-458a-abc1-bc5733bd787f)
    *This screenshot confirms that the cron package is installed if it appears in the list.*

2. **Install (if not installed):**
    ```bash
    sudo apt update
    sudo apt install cron
    ```

3. **Enable and start the service:**
    ```bash
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

4. **Check service status:**
    ```bash
    sudo systemctl status cron
    ```
    ![Checking cron service status: This screenshot displays the output of 'sudo systemctl status cron', confirming that the cron service is enabled and running.](https://github.com/user-attachments/assets/1e85d38c-5b2e-463c-9632-e827a65a07f5)

    *This screenshot shows that the cron service is active and running.*

---
# Basic Crontab Syntax Guide

The syntax of a cron job line in a crontab file uses the following format:

```
MIN HOUR DOM MON DOW CMD
```

- **MIN** – Minute (0–59)
- **HOUR** – Hour (0–23)
- **DOM** – Day of month (1–31)
- **MON** – Month (1–12 or JAN–DEC)
- **DOW** – Day of week (0–7 or SUN–SAT; both 0 and 7 mean Sunday)
- **CMD** – Command to execute (absolute path recommended)

Each field is separated by a single space. These five time fields tell Cron when to run the job.

## Example

To run a script on January 1st at 9 AM:

```
0 9 1 1 * /path/to/your_script.sh
```

The asterisk (`*`) means "every possible value" for that field.

---

## Cron Job Time Format Table

| Field            | Possible Values                   | Example         | Description                                           |
|------------------|-----------------------------------|-----------------|-------------------------------------------------------|
| Minute (MIN)     | 0 – 59                            | `7 * * * *`     | Runs when minute is 7 (e.g., 12:07, 13:07, etc.)      |
| Hour (HOUR)      | 0 – 23                            | `0 7 * * *`     | Runs at 7:00 AM (19:00 would be `0 19 * * *`)         |
| Day of Month     | 1 – 31                            | `0 0 7 * *`     | Runs on the 7th of every month at midnight            |
| Month (MON)      | 1 – 12 or JAN – DEC               | `0 0 0 7 *`     | Runs only in July (month = 7)                         |
| Day of Week (DOW)| 0 – 7 or SUN – SAT (0,7 = Sunday) | `0 0 * * 7`     | Runs only on Sundays                                  |

---

## Command to Execute

After the schedule, specify the absolute path to the script or command you want Cron to run.

## Cron Job Syntax and Examples

A crontab entry contains **five time fields** followed by the command to run:
```
*  *  *  *  *  command-to-run
|  |  |  |  |
|  |  |  |  └─ Day of the Week (0 - 7)
|  |  |  └──── Month (1 - 12)
|  |  └─────── Day of Month (1 - 31)
|  └────────── Hour (0 - 23)
└───────────── Minute (0 - 59)
```

**Common examples:**

| Schedule              | Cron Syntax       | Description                           |
|-----------------------|------------------|---------------------------------------|
| Every minute          | `* * * * *`      | Runs every minute                     |
| Every day at 2 AM     | `0 2 * * *`      | Runs daily at 2:00 AM                 |
| Every Monday at 5 PM  | `0 17 * * 1`     | Runs every Monday at 5:00 PM          |
| Every 15 minutes      | `*/15 * * * *`   | Runs every 15 minutes                 |

**Tip:** Use [crontab.guru](https://crontab.guru/) to build and check cron expressions.

---

## How to Create a Cron Job

### For the current user

1. **Open the crontab editor:**
    ```bash
    crontab -e
    ```
    *If this is your first time, you will be prompted to select a text editor:*

    ![Editor selection prompt: This screenshot displays the selection of a text editor for editing the crontab file. 'Nano' is recommended for beginners.](https://github.com/user-attachments/assets/0ff9c287-a0a9-49a7-a5ed-c0bb7e66c473)

    *This screenshot shows the editor selection menu. Beginners can select 'nano' for ease of use.*

2. **The editor will open the default crontab for your user:**

    ![Crontab editor: This screenshot shows the crontab file ready for editing, where you can add new cron job entries.](https://github.com/user-attachments/assets/58b92e6b-6e02-4c08-bdf2-f2498ebf1791)
    *This screenshot shows the crontab file open for editing. You can add, modify, or remove cron jobs here.*

3. **Add a cron job at the end of the file, for example:**
    ```bash
    5 * * * * /home/kawal/scripts/backup.sh
    ```
    *This schedules the script to run every hour at the 5th minute.*

---

## How to Edit or Remove Cron Jobs

- **View jobs:**
    ```bash
    crontab -l
    ```
    ![This screenshot shows the result of 'crontab -l', which lists all cron jobs scheduled for the current user.](https://github.com/user-attachments/assets/5edc4115-2bd8-466f-a2c4-a8c788d14d89)
    *Here, you see all jobs scheduled for the current user.*

- **Edit jobs:**
    ```bash
    crontab -e
    ```
    ![This screenshot shows the crontab open for editing. You can add, remove, or modify jobs here.](https://github.com/user-attachments/assets/336faba4-2014-4de0-a6b8-ff24af493c60)
    ![Editing jobs in crontab.](https://github.com/user-attachments/assets/5c6a2f02-1383-479c-9ca0-aa3f27e15143)
    ![Saving or removing jobs in crontab editor.](https://github.com/user-attachments/assets/e98f1c08-2066-4be3-8113-5c9c5e166756)
    
*These screenshots show the process of editing, saving changes, or deleting jobs in the crontab file.*

- **Remove all jobs:**
    ```bash
    crontab -r
    ```
    *This command deletes all cron jobs for the current user.*

- **Remove a specific job:**
    *Edit with `crontab -e` and delete the relevant line.*

---

## How to Check Cron Logs

- **Check if jobs ran:**
    ```bash
    grep CRON /var/log/syslog
    ```
    ![This screenshot shows the output of checking cron logs with 'grep CRON /var/log/syslog', which helps verify if your jobs ran as expected.](https://github.com/user-attachments/assets/6a51644e-27d9-45d2-964e-0d08ffa8ad14)
    *This screenshot helps verify job execution and troubleshoot errors.*

- **Or, using journalctl:**
    ```bash
    sudo journalctl -u cron
    ```

- **Log job output for troubleshooting:**
    ```bash
    0 2 * * * /home/kawal/scripts/backup.sh >> /home/kawal/logs/backup.log 2>&1
    ```
    *This example shows how to save all output (including errors) from a cron job to a log file for easier troubleshooting.*

---

## How to Restart Cron Service

If jobs aren’t running or changes aren’t taking effect:
```bash
sudo systemctl restart cron
```
*This command restarts the cron service. Use this if you suspect the service is stuck or not responding to changes.*

---

## Best Practices & Troubleshooting

- Test your scripts manually before using cron.
- Always use full (absolute) paths to scripts/commands.
- Redirect output to log files for easier debugging.
- Remember: cron runs with a minimal environment. Set necessary variables in your script.
- Backup your crontab before making changes:
    ```bash
    crontab -l > mycron.backup
    ```
- Review logs regularly to ensure jobs are executing as expected.

---

## Contact Information

| Name             | Email                                         |
|------------------|-----------------------------------------------|
| Kawalpreet Kour  | Kawalpreet.kour.snaatak@mygurukulam.co        |

---

## References

| Link                                                                 | Description                                   |
|----------------------------------------------------------------------|-----------------------------------------------|
| [YouTube: Document format](https://youtu.be/YA1wPJxeAmg?si=_oMBegTpPa3UfpIw) | Document format inspiration                  |
| [Cronitor: Cron Job Guide](https://cronitor.io/guides/cron-jobs#what-is-a-cron-job) | Cron basics and syntax                       |
| [Crontab Guru](https://crontab.guru/)                               | Build/check cron expressions                  |

---
