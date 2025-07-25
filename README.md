# SOP for Cron Jobs in Ubuntu

> This SOP provides a clear and structured guide to configuring and managing Cron Jobs on Ubuntu for automated task scheduling.

![Ubuntu Logo](https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png)

---
## Author Information
| Last Updated On | Version | Author           | Level           | Reviewer               |
|-----------------|---------|------------------|-----------------|------------------------|
| 18-07-2025      | V1.0    | Kawalpreet Kour  | Internal Review | Pritam                 |
| 25-07-2025      | V1.1    | Kawalpreet Kour  | L0              | Shreya/Sharvani        |
|                 |         | Kawalpreet Kour  | L1              | Abhishek V             |
|                 |         | Kawalpreet Kour  | L2              | Abhishek Dubey/Rishabh sharma |

---

## [Introduction](#introduction)

A cron job is a scheduled task in Unix-like systems that runs automatically at specified intervals (such as every minute, hour, day, or week). It is commonly used for automating repetitive tasks like backups, log rotation, or running scripts.

---

## Table of Contents

- [Purpose](#purpose)
- [Pre-requisites](#pre-requisites)
- [Cron Installation and Service Check](#cron-installation-and-service-check)
- [Basic Crontab Syntax Guide](#basic-crontab-syntax-guide)
  - [Crontab Field Format](#crontab-field-format)
  - [Cron Job Time Format Table](#cron-job-time-format-table)
  - [Symbols in Crontab](#symbols-in-crontab)
  - [Common Cron Examples](#common-cron-examples)
- [How to Create a Cron Job](#how-to-create-a-cron-job)
- [How to Edit or Remove Cron Jobs](#how-to-edit-or-remove-cron-jobs)
- [How to Check Cron Logs](#how-to-check-cron-logs)
- [How to Restart Cron Service](#how-to-restart-cron-service)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contact Information](#contact-information)
- [References](#references)

---

## [Purpose](#purpose)

To provide a clear, step-by-step guide on how to create, edit, and manage cron jobs, including checking logs and using correct syntax for scheduling tasks.

---

## [Pre-requisites](#pre-requisites)

| Heading               | Description                                                                                       |
|-----------------------|---------------------------------------------------------------------------------------------------|
| Basic Linux Knowledge | Comfortable with terminal usage and basic shell commands.                                         |
| Sudo Access           | Required to install packages, start/stop services, and edit system-wide configurations.           |
| Script Availability   | Shell scripts must already exist, be tested, and made executable before scheduling with cron.      |

---

## [Cron Installation and Service Check](#cron-installation-and-service-check)

1. *Check if cron is installed:*
    ```bash
    dpkg -l | grep cron
    ```
    ![Checking if cron is installed](https://github.com/user-attachments/assets/95a1ddd1-3c21-458a-abc1-bc5733bd787f)
    If 'cron' appears in the list, it means cron is installed on your system.

2. *Install (if not installed):*
    ```bash
    sudo apt update
    sudo apt install cron
    ```

3. *Enable and start the service:*
    ```bash
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

4. *Check service status:*
    ```bash
    sudo systemctl status cron
    ```
    ![Checking cron service status](https://github.com/user-attachments/assets/1e85d38c-5b2e-463c-9632-e827a65a07f5)
    This screenshot shows that the cron service is active and running.

---

## [Basic Crontab Syntax Guide](#basic-crontab-syntax-guide)

### [Crontab Field Format](#crontab-field-format)

A crontab entry contains *five time fields* followed by the command to run:

```
*  *  *  *  *  command-to-run
|  |  |  |  |
|  |  |  |  └─ Day of the Week (0 - 7)
|  |  |  └──── Month (1 - 12)
|  |  └─────── Day of Month (1 - 31)
|  └────────── Hour (0 - 23)
└───────────── Minute (0 - 59)
```

---

### [Cron Job Time Format Table](#cron-job-time-format-table)

| Field            | Possible Values                   | Example         | Description                                           |
|------------------|-----------------------------------|-----------------|-------------------------------------------------------|
| Minute (MIN)     | 0 – 59                            | 7 * * * *       | Runs when minute is 7 (e.g., 12:07, 13:07, etc.)      |
| Hour (HOUR)      | 0 – 23                            | 0 7 * * *       | Runs at 7:00 AM (19:00 would be 0 19 * * *)           |
| Day of Month     | 1 – 31                            | 0 0 7 * *       | Runs on the 7th of every month at midnight            |
| Month (MON)      | 1 – 12 or JAN – DEC               | 0 0 0 7 *       | Runs only in July (month = 7)                         |
| Day of Week (DOW)| 0 – 7 or SUN – SAT (0,7 = Sunday) | 0 0 * * 7       | Runs only on Sundays                                  |

---

### [Symbols in Crontab](#symbols-in-crontab)

| Symbol    | Description                        | Example           | Meaning                                 |
|-----------|------------------------------------|-------------------|-----------------------------------------|
| *         | Every possible value (wildcard)    | * * * * *         | Every minute                            |
| ,         | Value list separator               | 1,15,30 * * * *   | At 1st, 15th, and 30th minute           |
| -         | Range of values                    | 1-5 * * * *       | Every minute from 1 to 5                |
| /         | Step values                        | */10 * * * *      | Every 10 minutes (0,10,20,...)          |
| @reboot   | Run once at startup                | @reboot CMD       | Executes command on system boot         |

---

### [Common Cron Examples](#common-cron-examples)

| Schedule              | Cron Syntax       | Description                   |
|-----------------------|------------------|-------------------------------|
| Every minute          | * * * * *        | Runs every minute             |
| Every day at 2 AM     | 0 2 * * *        | Runs daily at 2:00 AM         |
| Every Monday at 5 PM  | 0 17 * * 1       | Runs every Monday at 5:00 PM  |
| Every 15 minutes      | */15 * * * *     | Runs every 15 minutes         |

*Tip:* Use [crontab.guru](https://crontab.guru/) to build and check cron expressions.

---

## [How to Create a Cron Job](#how-to-create-a-cron-job)

### For the current user

1. *Open the crontab editor:*
    ```bash
    crontab -e
    ```
    If this is your first time, you will be prompted to select a text editor:

    ![Editor selection prompt](https://github.com/user-attachments/assets/0ff9c287-a0a9-49a7-a5ed-c0bb7e66c473)
     
    Beginners can select 'nano' for ease of use.

2. *The editor will open the default crontab for your user:*

    ![Crontab editor](https://github.com/user-attachments/assets/58b92e6b-6e02-4c08-bdf2-f2498ebf1791)
    You can add, modify, or remove cron jobs here.

3. *Add a cron job at the end of the file, for example:*
    ```bash
    5 * * * * /home/kawal/scripts/backup.sh
    ```
    This schedules the script to run every hour at the 5th minute.

---

## [How to Edit or Remove Cron Jobs](#how-to-edit-or-remove-cron-jobs)

- *View jobs:*
    ```bash
    crontab -l
    ```
    ![List cron jobs](https://github.com/user-attachments/assets/5edc4115-2bd8-466f-a2c4-a8c788d14d89)
    Lists all cron jobs scheduled for the current user.

- *Edit jobs:*
    ```bash
    crontab -e
    ```
    ![Edit crontab](https://github.com/user-attachments/assets/336faba4-2014-4de0-a6b8-ff24af493c60)
    ![Editing jobs in crontab](https://github.com/user-attachments/assets/5c6a2f02-1383-479c-9ca0-aa3f27e15143)
    ![Saving or removing jobs in crontab editor](https://github.com/user-attachments/assets/e98f1c08-2066-4be3-8113-5c9c5e166756)
    
    Process of editing, saving changes, or deleting jobs in the crontab file.

- *Remove all jobs:*
    ```bash
    crontab -r
    ```
    <img width="578" height="110" alt="Screenshot from 2025-07-18 14-41-03" src="https://github.com/user-attachments/assets/ea337a8d-2630-407b-981a-97fb869c81f2" />
    
    This command deletes all cron jobs for the current user.

- *Remove a specific job:*
    Edit with `crontab -e` and delete the relevant line.

---

## [How to Check Cron Logs](#how-to-check-cron-logs)

- *Check if jobs ran:*
    ```bash
    grep CRON /var/log/syslog
    ```
    ![Cron logs check](https://github.com/user-attachments/assets/6a51644e-27d9-45d2-964e-0d08ffa8ad14)
    Helps verify job execution and troubleshoot errors.

- *Or, using journalctl:*
    ```bash
    sudo journalctl -u cron
    ```

- *Log job output for troubleshooting:*
    ```bash
    0 2 * * * /home/kawal/scripts/backup.sh >> /home/kawal/logs/backup.log 2>&1
    ```
    Save all output (including errors) from a cron job to a log file for easier troubleshooting.

---

## [How to Restart Cron Service](#how-to-restart-cron-service)

If jobs aren’t running or changes aren’t taking effect:
```bash
sudo systemctl restart cron
```
Restarts the cron service if you suspect it's stuck or not responding to changes.

---

## Best Practices

| Practice                  | Description                                                                                          |
|---------------------------|------------------------------------------------------------------------------------------------------|
| Use Absolute Paths        | Always use full paths to commands and scripts (e.g., /home/kawal/crontest.sh, not just crontest.sh).|
| Redirect Output to Logs   | Send both standard output and errors to log files using >> logfile 2>&1.                             |
| Test Scripts Manually     | Run your scripts manually to ensure they work as expected before adding them to crontab.             |
| Backup Cron Jobs          | Regularly back up your crontab using: crontab -l > mycron.backup.                                   |

---

## [Troubleshooting](#troubleshooting)

| Issue                       | Cause                        | Solution                                              |
|-----------------------------|------------------------------|-------------------------------------------------------|
| Cron job not executing      | Wrong path or syntax error   | Use full paths, test the command manually             |
| No output or error logs     | Output not redirected        | Add >> /path/to/log 2>&1 at end of command            |
| Environment vars missing    | Cron uses limited environment| Define needed variables inside the script             |
| Script works manually only  | Path/user mismatch           | Check permissions, use #!/bin/bash, full paths        |
| Cron service not running    | Service is disabled          | Start it using sudo systemctl start cron              |

---

## [Contact Information](#contact-information)

| Name             | Email                                         |
|------------------|-----------------------------------------------|
| Kawalpreet Kour  | Kawalpreet.kour.snaatak@mygurukulam.co        |

---

## [References](#references)

| Links                                                                                     | Descriptions                           |
|-------------------------------------------------------------------------------------------|----------------------------------------|
| [Crontab Guru](https://crontab.guru)                                                      | Build and verify cron expressions      |
| [Cronitor Docs](https://cronitor.io/docs)                                                 | Guide on cron job scheduling basics    |
| [YouTube: Cron Job Tutorial (Ubuntu)](https://youtu.be/YA1wPJxeAmg?si=5-wPYkb9KV2lhZcx)   | Video guide to setup cron jobs         |

---
