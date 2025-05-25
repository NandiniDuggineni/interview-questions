# interview-questions
Real time interview questions

BASH
1. How do you create and run a Bash script in Linux?
Creating and running a Bash script in Linux involves three main steps:
Step 1: Create the Bash Script File - nano myscript.sh, Inside the file, write your Bash script. #!/bin/bash - echo "Hello, World!"
#!/bin/bash is called a shebang. It tells the system to use the Bash shell to interpret the script.
In nano, press CTRL + O to write (save), then CTRL + X to exit.
Step 2: Make the Script Executable - chmod +x myscript.sh
Step 3: Run the Script - You can run the script in two ways: 1. Using a relative or absolute path: ./myscript.sh and 2. Using bash to interpret it: bash myscript.sh

2. What are $0, $1, $@, and $# in a Bash script? ./myscript.sh foo "bar baz" 123
#!/bin/bash
echo "Script name: $0"
echo "Total arguments: $#"
echo "First argument: $1"
echo "All arguments:"
for arg in "$@"; do
  echo "$arg"
done

Script name: ./myscript.sh
Total arguments: 3
First argument: foo
All arguments:
foo
bar baz
123

3. What is the difference between " and ' in Bash?
 Use when you want Bash to interpret variables or commands inside the string.
NAME="Alice"
echo "Hello, $NAME"
output: Hello, Alice
Everything is literal inside single quotes.
NAME="Alice"
echo 'Hello, $NAME'
output: Hello, $NAME

4. How do you handle errors in a Bash script?
5. Explain how to use conditionals (if, elif, else) in Bash.
#!/bin/bash
NUM=10
if [ $NUM -gt 0 ]; then
  echo "Number is positive"
elif [ $NUM -lt 0 ]; then
  echo "Number is negative"
else
  echo "Number is zero"
fi
-eq(equal to) ,-lt ( less than ) ,-gt (grater than ) ,-le( less than equal to) ,-ge ( grater than equal to),!= (Not equal to) ,-n (not empty) ,-z (empty), -f ( file), -d(directory), =, ==
FILE="myfile.txt"

if [ -f "$FILE" ]; then
  echo "$FILE exists."
else
  echo "$FILE does not exist."
fi

[ -f "$FILE" ] && echo "File exists" || echo "File missing"


6. Write a script to check if a service (e.g., nginx) is running and restart it if it is not.
#!/bin/bash

SERVICE="nginx"

# Check if the service is active (running)
if systemctl is-active --quiet $SERVICE; then
  echo "$SERVICE is running."
else
  echo "$SERVICE is NOT running. Restarting $SERVICE..."
  # Try to restart the service
  sudo systemctl restart $SERVICE

  # Check if restart was successful
  if systemctl is-active --quiet $SERVICE; then
    echo "$SERVICE restarted successfully."
  else
    echo "Failed to restart $SERVICE. Please check the service status manually."
  fi
fi

7. How would you write a script to rotate logs and keep only the last 7 days?
Copies app.log to a file with today's date appended, e.g., app.log.2025-05-24.
Clears (truncate) the original log file to start fresh.
Uses find to delete backup logs older than 7 days.
Prints a confirmation message.
#!/bin/bash
# rotate_logs.sh

LOG_DIR="/var/log/myapp"
LOG_FILE="app.log"
BACKUP_PREFIX="app.log."

# Number of days to keep logs
DAYS_TO_KEEP=7

# Create a dated backup of current log
DATE=$(date +%Y-%m-%d)
cp "$LOG_DIR/$LOG_FILE" "$LOG_DIR/${BACKUP_PREFIX}${DATE}"

# Truncate the current log file (clear its content)
: > "$LOG_DIR/$LOG_FILE"

# Delete backups older than DAYS_TO_KEEP
find "$LOG_DIR" -name "${BACKUP_PREFIX}*" -type f -mtime +$DAYS_TO_KEEP -exec rm -f {} \;

echo "Logs rotated. Keeping last $DAYS_TO_KEEP days of backups."

chmod +x rotate_logs.sh
crontab -e
0 0 * * * /path/to/rotate_logs.sh

8. Write a Bash script to monitor disk space and send an alert if usage exceeds 80%.
#!/bin/bash

# Partition to monitor
PARTITION="/"

# Threshold percentage (without % sign)
THRESHOLD=80

# Get current disk usage percentage (without %)
USAGE=$(df -h "$PARTITION" | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$USAGE" -ge "$THRESHOLD" ]; then
  echo "Alert: Disk usage on $PARTITION is ${USAGE}% which exceeds the threshold of ${THRESHOLD}%."
fi

if we want to send a mail on alert
SUBJECT="Disk Space Alert"
MESSAGE="Warning: disk usage is over 80%.\nTake action!"
EMAIL="admin@example.com"

echo -e "$MESSAGE" | mail -s "$SUBJECT" "$EMAIL"

9. Explain how cron works. How would you schedule a script to run every 5 minutes?
crontab - e
* * * * * command-to-run
│ │ │ │ │
│ │ │ │ └── Day of the week (0 - 7) [Sunday is 0 or 7]
│ │ │ └──── Month (1 - 12)
│ │ └────── Day of the month (1 - 31)
│ └──────── Hour (0 - 23)
└────────── Minute (0 - 59)

10. Write a script to list all files larger than 100MB in /var/log and archive them.
#!/bin/bash

# Create archive with files > 100MB in /var/log
find /var/log -type f -size +100M > files_to_archive.txt

# List found files
cat files_to_archive.txt

# Archive them
tar -czf large_logs.tar.gz -T files_to_archive.txt

# Clean up
rm files_to_archive.txt

echo "Archived to large_logs.tar.gz"

11. How would you use python to list all running EC2 instances using the AWS CLI?





