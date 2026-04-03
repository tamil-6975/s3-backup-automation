#  Automated Log Backup to AWS S3 using Bash

A Bash automation script that **zips log files** from an AWS EC2 instance and **uploads them to AWS S3** daily using cron scheduling. This project demonstrates real-world DevOps skills including cloud storage, IAM security, shell scripting, and Linux automation.

---

## Project Overview

```
EC2 Instance (Amazon Linux 2)
        ↓
Bash Script (zips log files)
        ↓
AWS S3 Bucket (stores backups safely)
        ↓
Cron Job (runs automatically every day)
```

---

##  Tech Stack

| Technology | Purpose |
|-----------|---------|
| AWS EC2 (t2.micro) | Cloud virtual server |
| AWS S3 | Cloud storage for backups |
| AWS IAM | Secure role-based access |
| Amazon Linux 2 | Operating system |
| Bash Scripting | Automation script |
| Cron | Task scheduling |
| AWS CLI | Upload files to S3 |

---

##  Features

- Automatically zips log files using `tar`
- Uploads compressed backup to AWS S3 daily
- Deletes local backup after upload to save disk space
- Logs all backup activity to a log file
- Threshold-based error handling with success/failure messages
- Scheduled via cron to run every day at midnight
- Uses IAM Role for secure, credential-free S3 access

---

##  Project Structure

```
s3-backup/
│
├── s3-backup.sh      # Main backup automation script
└── README.md         # Project documentation
```

---

##  Setup Instructions

### Step 1 — Launch EC2 Instance
- Go to AWS Console → EC2 → Launch Instance
- AMI: Amazon Linux 2023 (Free tier)
- Instance type: t2.micro (Free tier)

### Step 2 — Create S3 Bucket
1. Go to **AWS Console → S3**
2. Click **Create bucket**
3. Bucket name: `my-ec2-backups-tamil`
4. Region: Same as EC2 (e.g. us-east-1)
5. Click **Create bucket**

### Step 3 — Create IAM Role
1. Go to **AWS Console → IAM → Roles**
2. Click **Create role**
3. Select **AWS Service → EC2**
4. Attach policy: **AmazonS3FullAccess**
5. Role name: `EC2-S3-Backup-Role`
6. Click **Create role**

### Step 4 — Attach IAM Role to EC2
1. Go to **EC2 → Instances**
2. Select your instance
3. Click **Actions → Security → Modify IAM Role**
4. Select `EC2-S3-Backup-Role`
5. Click **Update IAM Role**

### Step 5 — Connect to EC2
```bash
ssh -i my-key.pem ec2-user@<YOUR-EC2-PUBLIC-IP>
```

### Step 6 — Create Project Folder
```bash
mkdir -p ~/projects/s3-backup
cd ~/projects/s3-backup
```

### Step 7 — Create and Run the Script
```bash
# Create script
nano s3-backup.sh

# Make executable
chmod +x s3-backup.sh

# Run manually
./s3-backup.sh
```

### Step 8 — Install and Configure Cron
```bash
# Install cron
sudo yum install cronie -y

# Start cron service
sudo systemctl start crond
sudo systemctl enable crond

# Add cron job (runs daily at midnight)
crontab -e
```

Add this line:
```bash
0 0 * * * /home/ec2-user/projects/s3-backup/s3-backup.sh >> /var/log/s3_backup.log 2>&1
```

---

##  Useful Commands

```bash
# Run backup manually
./s3-backup.sh

# View backup logs
cat /var/log/s3_backup.log

# List all backups in S3
aws s3 ls s3://my-ec2-backups-tamil/backups/

# Download a backup from S3
aws s3 cp s3://my-ec2-backups-tamil/backups/backup-2026-04-03.tar.gz .

# View cron jobs
crontab -l

# Check cron service status
sudo systemctl status crond
```

---

##  AWS Services Used

| Service | Usage |
|---------|-------|
| EC2 | Virtual server to run the backup script |
| S3 | Cloud storage for backup files |
| IAM Role | Secure access from EC2 to S3 |
| AWS CLI | Command line tool to upload to S3 |
| CloudWatch (future) | Monitor backup success/failure |

---

##  Sample Output

```
Starting backup - 2026-04-03
Files zipped: backup-2026-04-03.tar.gz
Uploaded to S3: s3://my-ec2-backups-tamil/backups/backup-2026-04-03.tar.gz
Local backup deleted (saved to S3)

All backups in S3:
2026-04-03  backup-2026-04-03.tar.gz

Backup completed successfully - 2026-04-03
```

---

## Security

- EC2 accesses S3 using **IAM Role** — no hardcoded credentials
- IAM Role follows **least privilege principle**
- Backup files are stored privately in S3

---

## Future Improvements

- [ ] Add S3 lifecycle policy to auto-delete old backups
- [ ] Send email notification on backup failure using SNS
- [ ] Encrypt backup files using AWS KMS
- [ ] Monitor backup jobs using CloudWatch
- [ ] Store multiple versions using S3 versioning

---

## Resume Bullet Points

- Automated daily log backups from AWS EC2 to S3 using Bash scripting and cron scheduling
- Configured AWS IAM Role with S3FullAccess policy for secure, credential-free EC2 to S3 access
- Implemented error handling and logging in Bash script to track backup success and failures
- Reduced manual backup effort by 100% through full automation using Linux cron jobs

---

## Author

**tamil-6975**
- GitHub: [github.com/tamil-6975](https://github.com/tamil-6975)

---

## License

This project is open source and available under the [MIT License](LICENSE).
