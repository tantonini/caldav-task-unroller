# CalDAV Task Unroller

A "Monitor & Archive" script for CalDAV recurring tasks.

## The Problem
Native CalDAV clients (like Tasks.org, Nextcloud, or Apple Reminders) handle recurring tasks by simply updating the `DUE` date when you complete an occurrence. While efficient, this behavior **destroys the history** of your completed tasks. You have no way of knowing when (or if) you completed a routine task in the past.

## The Solution
**CalDAB Task Unroller** acts as a silent archivist. 

It periodically monitors your task list and remembers the state of your recurring tasks. When you check off a recurring task in your app, its due date advances. The script detects this change, calculates the exact moment you completed it, and generates a standalone, `COMPLETED` task in your history. 

Your daily task list remains clean (no duplicates), but your history is perfectly preserved.

## Setup
1. Clone this repository.
2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Copy the example configuration:
   ```bash
   cp config.example.json config.json
   ```
4. Edit `config.json` with your CalDAV credentials.
5. Make the script executable:
   ```bash
   chmod +x caldav-task-unroller
   ```

## Configuration
* `caldav_url`: Your CalDAV server endpoint (e.g., Nextcloud's `remote.php/dav`).
* `caldav_username`: Your account username.
* `caldav_password`: Your password (it is highly recommended to use a dedicated App Password/Token).
* `calendar_name`: The exact name of the task list/calendar you want to monitor.
* `state_file`: Local JSON file used as the script's memory (default: `state.json`).

## Usage
The script is designed to be run periodically, ideally via a cron job. 

For example, to run the script every hour:
```bash
0 * * * * cd /path/to/caldav-task-unroller && ./caldav-task-unroller
```
