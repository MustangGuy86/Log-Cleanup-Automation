# Log-Cleanup-Automation

Project Description

This project involves creating a Python script to automatically clean up old log files from a specified directory. The script ensures that only the most recent log files are retained, removing those that exceed a specified retention period. Additionally, it generates a summary of deleted files for record-keeping.
Preliminary Steps

Before starting with the script setup, ensure your VM is prepared and configured correctly.
1. Prepare the VM

    Update the System

    Ensure your VM is up-to-date with the latest packages and security updates:

    bash

sudo apt update
sudo apt upgrade -y

Install Python 3

Check if Python 3 is installed. If not, install it:

bash

python3 --version
sudo apt install python3 python3-pip -y

Create Required Directories

Set up the directories for logs and the summary file:

bash

mkdir -p /home/cgoetz/logs

Ensure you have write permissions for this directory:

bash

chmod 755 /home/cgoetz/logs

Create a Virtual Environment (Optional)

For isolating dependencies, you may want to create a Python virtual environment:

bash

python3 -m venv myenv
source myenv/bin/activate

Install Required Python Packages

Install any necessary Python packages (though for this script, no additional packages are needed):

bash

    pip install psutil

Steps to Set Up and Test the Project
1. Setup the Python Script

    Create the Script

    Create a Python script named log_cleanup.py with the following content:

    python

import os
import time

LOG_DIR = '/home/cgoetz/logs'
RETENTION_DAYS = 7
SUMMARY_FILE = '/home/cgoetz/cleanup_summary.txt'

def delete_old_logs():
    current_time = time.time()
    cutoff_time = current_time - (RETENTION_DAYS * 86400)
    deleted_files = []

    for filename in os.listdir(LOG_DIR):
        file_path = os.path.join(LOG_DIR, filename)
        if os.path.isfile(file_path):
            file_mtime = os.path.getmtime(file_path)
            if file_mtime < cutoff_time:
                os.remove(file_path)
                deleted_files.append(filename)

    with open(SUMMARY_FILE, 'w') as summary_file:
        summary_file.write(f"Deleted files:\n")
        for file in deleted_files:
            summary_file.write(f"{file}\n")

if __name__ == "__main__":
    delete_old_logs()

Ensure Script Permissions

Make sure the script has the necessary permissions to execute:

bash

    chmod +x /home/cgoetz/log_cleanup.py

2. Test the Script

    Run the Script Manually

    Execute the script to ensure it works correctly:

    bash

python3 /home/cgoetz/log_cleanup.py

Check the log directory and the summary file:

bash

ls -lh /home/cgoetz/logs
cat /home/cgoetz/cleanup_summary.txt

Verify that old files have been removed and that the summary file reflects the correct information.

Verify Permissions

Confirm that the script has the right permissions:

bash

ls -l /home/cgoetz/logs
ls -l /home/cgoetz/cleanup_summary.txt

Ensure the script has write permissions for the directory and the summary file.

Schedule with Cron

To automate the script, schedule it to run periodically using cron:

bash

crontab -e

Add the following line to run the script daily at midnight:

bash

0 0 * * * /usr/bin/python3 /home/cgoetz/log_cleanup.py

Confirm the cron job is set up correctly:

bash

crontab -l

You can also manually execute the cron job command to test:

bash

    /usr/bin/python3 /home/cgoetz/log_cleanup.py

    Monitor the log directory and summary file for accuracy.

3. Handle Issues

    FileNotFoundError: If you encounter a FileNotFoundError, ensure that the specified log directory and summary file paths exist and are correctly set in the script.

    Permissions Errors: If there are permission issues, verify and adjust the permissions of the log directory and summary file as needed.

Conclusion

This project demonstrates a fundamental skill in automation and file management using Python. By automating log cleanup, it showcases your ability to manage system resources efficiently and maintain a clean and organized logging environment. The project also highlights your knowledge of Python scripting, file operations, and scheduling tasks using cron.
