#!/bin/bash

# ============================================================
#  PROJECT 3: S3 Backup Automation
#  Description: Zips log files and uploads to AWS S3
#  Schedule: Runs daily via cron
# ============================================================

# ---------- CONFIG ----------
S3_BUCKET="s3://my-ec2-backups-tamil"
BACKUP_DIR="/home/ec2-user/backups"
LOG_SOURCE="/var/log/system_monitor"
DATE=$(date +%Y-%m-%d)
BACKUP_FILE="backup-$DATE.tar.gz"
# ----------------------------

# Step 1 — Create backup folder
mkdir -p "$BACKUP_DIR"

echo "🔄 Starting backup - $DATE"

# Step 2 — Zip the log files
tar -czf "$BACKUP_DIR/$BACKUP_FILE" "$LOG_SOURCE"

if [ $? -eq 0 ]; then
  echo "✅ Files zipped: $BACKUP_FILE"
else
  echo "❌ Zipping failed!"
  exit 1
fi

# Step 3 — Upload to S3
aws s3 cp "$BACKUP_DIR/$BACKUP_FILE" "$S3_BUCKET/backups/$BACKUP_FILE"

if [ $? -eq 0 ]; then
  echo "✅ Uploaded to S3: $S3_BUCKET/backups/$BACKUP_FILE"
else
  echo "❌ Upload to S3 failed!"
  exit 1
fi

# Step 4 — Delete local backup (save EC2 disk space)
rm "$BACKUP_DIR/$BACKUP_FILE"
echo "🗑️  Local backup deleted (saved to S3)"

# Step 5 — List all backups in S3
echo ""
echo "📦 All backups in S3:"
aws s3 ls "$S3_BUCKET/backups/"

echo ""
echo "✅ Backup completed successfully - $DATE"
