# Log Cleanup Automation

## Project Overview

This project involves creating a Python script to automate the cleanup of old log files and scheduling it to run periodically using cron. It demonstrates skills in Python scripting, file management, and task automation.

## Preliminary Steps

Before starting with the script setup, ensure your VM is prepared and configured correctly.

1. **Prepare the VM**

    **Update the System**
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

    **Install Python 3**
    ```bash
    python3 --version
    sudo apt install python3 python3-pip -y
    ```

    **Create Required Directories**
    ```bash
    mkdir -p /home/cgoetz/logs
    chmod 755 /home/cgoetz/logs
    ```

    **Create a Virtual Environment (Optional)**
    ```bash
    python3 -m venv myenv
    source myenv/bin/activate
    ```

    **Install Required Python Packages**
    ```bash
    pip install psutil
    ```

## Setup and Test

1. **Create the Script**

    Create a Python script named `log_cleanup.py` with the following content:
    ```python
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
    ```

2. **Ensure Script Permissions**
    ```bash
    chmod +x /home/cgoetz/log_cleanup.py
    ```

3. **Run the Script Manually**
    ```bash
    python3 /home/cgoetz/log_cleanup.py
    ```

4. **Check the Log Directory and Summary File**
    ```bash
    ls -lh /home/cgoetz/logs
    cat /home/cgoetz/cleanup_summary.txt
    ```

5. **Verify Permissions**
    ```bash
    ls -l /home/cgoetz/logs
    ls -l /home/cgoetz/cleanup_summary.txt
    ```

## Schedule with Cron

To automate the script, schedule it to run periodically using cron:

1. **Edit Crontab**
    ```bash
    crontab -e
    ```

2. **Add Cron Job**
    ```bash
    0 0 * * * /usr/bin/python3 /home/cgoetz/log_cleanup.py
    ```

3. **Confirm Cron Job**
    ```bash
    crontab -l
    ```

4. **Test the Cron Job Command**
    ```bash
    /usr/bin/python3 /home/cgoetz/log_cleanup.py
    ```

## Handle Issues

- **FileNotFoundError**: If you encounter a `FileNotFoundError`, ensure that the specified log directory and summary file paths exist and are correctly set in the script.
- **Permissions Errors**: If there are permission issues, verify and adjust the permissions of the log directory and summary file as needed.

## Conclusion

This project demonstrates a fundamental skill in automation and file management using Python. By automating log cleanup, it showcases your ability to manage system resources efficiently and maintain a clean and organized logging environment. The project highlights your knowledge of Python scripting, file operations, and scheduling tasks using cron.
