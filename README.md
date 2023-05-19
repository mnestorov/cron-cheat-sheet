# Cron Cheat Sheet

Cron is a job scheduler in Unix-like operating systems. Users can schedule jobs (commands or scripts) to run at specific times on specific days.

A cron job is an entry in a cron table (crontab), which is a list of all the jobs that cron will check and run if necessary.

## Basic Format of a Cron Job

**A cron job line consists of 6 components:**

```
*   *   *   *   *      command_to_be_executed
-   -   -   -   -
|   |   |   |   |
|   |   |   |   +----- day of the week (0 - 6) (Sunday=0)
|   |   |   +--------- month (1 - 12)
|   |   +------------- day of the month (1 - 31)
|   +----------------- hour (0 - 23)
+--------------------- min (0 - 59)
```

## Special Characters

- **Asterisk (*):** any value
- **Comma (,):** value list separator
- **Dash (-):** range of values
- **Slash (/):** steps values

## Special Strings

**Instead of the first five fields, one of eight special strings can be used:**

- **@reboot:** Run once, at startup
- **@yearly:** Run once a year, "0 0 1 1 *"
- **@annually:** Same as @yearly
- **@monthly:** Run once a month, "0 0 1 * *"
- **@weekly:** Run once a week, "0 0 * * 0"
- **@daily:** Run once a day, "0 0 * * *"
- **@hourly:** Run once an hour, "0 * * * *"

## Special String Examples

### A script that is run once at system boot

```
@reboot /path/to/command
```

### A script that runs once an hour

```
@hourly /path/to/command
```

### A script that runs once a day

```
@daily /path/to/command
```

### A script that runs once a week

```
@weekly /path/to/command
```

### A script that runs once a month

```
@monthly /path/to/command
```

### A script that runs once a year

```
@yearly /path/to/command
```

## Example Cron Jobs

### A script that runs every minute

```
* * * * * /path/to/command
```

### A script that runs every five minutes

```
*/5 * * * * /path/to/command
```

### A script that runs at the top of every hour

```
0 * * * * /path/to/command
```

### A script that runs every 5 hours

```
0 */5 * * * /path/to/command
```

### A script that runs at midnight every day

```
0 0 * * * /path/to/command
```

### A job that runs at 2am every day

```
0 2 * * * /path/to/command
```

### A job that runs every 15 minutes

```
*/15 * * * * /path/to/command
```

### A script that runs at 5 PM on weekdays (Monday through Friday)

```
0 17 * * 1-5 /path/to/command
```

### A job that runs at 7:30pm on weekdays (Monday through Friday)

```
30 19 * * 1-5 /path/to/command
```

### A script that runs at 9:30 AM on every Sunday

```
30 9 * * 0 /path/to/command
```

### A script that runs at 8:00 AM on the first day of every month

```
0 8 1 * * /path/to/command
```

### A job that runs at 6:15pm on the last day of every month

```
15 18 * * * [ "$(date +\%m -d tomorrow)" != "$(date +\%m)" ] && /path/to/command
```

### Backup the website every day at 2:30am

```
30 2 * * * /path/to/backup_command
```

## Crontab Entries

### Edit Entries

**To edit the list of cronjobs you can run:**

```
crontab -e
```

### List Entries

**To view the list of existing cronjobs:**

```
crontab -l
```

### Remove All Cron Jobs

**If you want to remove all cron jobs, you can use the following command:**

```
crontab -r
```

**Note:** Remember, this will remove all your cron jobs.

## Best Practices and Tips

1. **_Test Your Commands:_** Before adding a job to your crontab, always test the command or script in your terminal. This will help you catch and resolve any errors that may prevent the job from running as expected.
2. **_Manage Output:_** Cron jobs can generate a lot of output, especially if they're run often. By default, this output is mailed to the user the job runs under. To prevent this, you can redirect the output to a log file, a different mail address, or discard it completely. For example, to discard both standard output and errors, append >/dev/null 2>&1 to your command.
3. **_Consider Time Zones:_** By default, Cron uses the system's time zone. If your job should run at specific times in a different time zone, you'll need to handle this in your command or script. Be aware that Daylight Saving Time can complicate this further.
4. **_Don't Assume Your Environment:_** Cron jobs run with a limited set of environment variables, which may not be the same as those available in an interactive terminal session. Always use absolute paths for commands and files to avoid this issue.
5. **_Use Lock Files for Long-Running Jobs:_** If a job could run longer than the interval between its starts, use a lock file to prevent the job from running multiple times concurrently. The flock command can help with this.
6. **_Use Appropriate Scheduling:_** Be aware of the load you're putting on your system with your cron jobs. Try to schedule resource-intensive jobs for off-peak times.
7. **_Use Comments:_** Use comments in your crontab to describe what each job does and when it's supposed to run. This can make troubleshooting easier.
8. **_Use Monitoring:_** Consider using a monitoring tool to alert you if a job fails to run, runs for too long, or produces certain output. Some examples are Cronitor, Healthchecks.io, and Dead Man's Snitch.
9. **_Backup Your Crontab:_** There's no automatic versioning or backup of your crontab. Consider keeping a copy of it under version control.
10. **_Specific Timing:_** When scheduling, be aware that using both day of month and day of week fields together can lead to confusing behavior, because this will be interpreted as an OR operation, not an AND operation.
11. **_Testing Cron Timing:_** For testing the time of running cron jobs, instead of waiting you can adjust your system time.
12. **_Leverage More Advanced Scheduling Systems:_** If your requirements are becoming too complex for Cron, consider using a more advanced job scheduling system. Options include system-level replacements like anacron and systemd timers, and application-level job queues like those provided by Celery or Sidekiq.

**Remember:** Cron is a powerful tool, but it requires careful use and monitoring. Always thoroughly test new cron jobs, and keep an eye on them once they're live.

---

## License

This project is released under the MIT License.
