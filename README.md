# CalDAV Task Unroller

A "Monitor & Archive" script for CalDAV recurring tasks.

## The Problem
Native CalDAV clients (like Tasks.org, Nextcloud, or Apple Reminders) handle recurring tasks by advancing the `DUE` date when an occurrence is checked off. While practical, this behavior **destroys the history** of completed tasks—making it impossible to track when or how often routine tasks were actually performed.

## The Solution
**Task Unroller** operates as an automated archivist on a **single calendar**:

1. **Monitor:** It periodically scans your task list for recurring tasks (`RRULE`) and tracks their due dates in a local state file (`state.json`).
2. **Detect:** When you complete a task in your app, its due date shifts forward. On its next run, the script detects this date advancement.
3. **Archive:** The script automatically generates a standalone, `COMPLETED` task in your history matching the original due date and the exact completion timestamp (`LAST-MODIFIED`).

Your daily task list stays clean without visual duplicates, while your historical log remains accurate.

## Setup

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/your-username/caldav-task-unroller.git](https://github.com/your-username/caldav-task-unroller.git)
   cd caldav-task-unroller
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure credentials:**
   ```bash
   cp config.example.json config.json
   ```
   Edit `config.json` with your CalDAV server details and target calendar name.

4. **Make executable:**
   ```bash
   chmod +x caldav-task-unroller
   ```

## Configuration Options

Edit `config.json` to suit your setup:

* `caldav_url`: Your CalDAV endpoint URL (e.g., `https://nextcloud.example.com/remote.php/dav`).
* `caldav_username`: Your account username.
* `caldav_password`: Your password or App Password / Token.
* `calendar_name`: The display name of the task list/calendar to monitor.
* `state_file`: Path to the JSON state file (default: `state.json`).

## Usage & CLI Flags

Run the script directly or via cron:

```bash
# Standard execution
./caldav-task-unroller

# Simulate execution without modifying CalDAV or state.json
./caldav-task-unroller --dry-run

# Specify a custom configuration file
./caldav-task-unroller --config /path/to/custom_config.json
```

### CLI Arguments

* `-d`, `--dry-run`: Enables simulation mode. Scans tasks and prints what would be archived without pushing changes to CalDAV or updating `state.json`.
* `-c`, `--config`: Specifies a custom path for the configuration JSON file.

## Automated Execution (Cron)

To run the script automatically every hour:

```cron
0 * * * * cd /path/to/caldav-task-unroller && ./caldav-task-unroller > /dev/null 2>&1
```
