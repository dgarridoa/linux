# Processes and Job Control
- **Foreground Processes**: processes that are running in the shell and is not possible type in the shell until the processes end (are running in a first plane).
  - `command ...`
  - `Ctrl-c`: kill the foreground process.
  - `Ctrl-z`: suspend the foreground process.
  - `fg [%num]`: foreground a background process.
- **Background Processes**: processes that are running in a second plane and allow type in the shell while the process is running.
  - `command & ...`
  - `bg [%num]`: background a suspended process.
- `ps`: display process status.
  - `-e`: everything, all processes.
  - `-f`: full format listing.
  - `-u username`: display username's processes.
  - `-p pid`: display information for PID (processes id).
  - `-H or --forest`: display a process tree.
- `pstree`: display processes in a tree format.
- `top`: interactive processes viewer.
- `htop`: interactive processes viewer, similar to top, but with additional features.
- `kill [-sig] pid or [%num]`: kill a process by job number or PID.
  - `-l`: display a list of signals
- `jobs [%num]`: list jobs.

# Scheduling Repeated Jobs with Cron

Use cron to schedule and automate tasks.
 - `uptime`: ask the time during which the machine is running.
- `cron`: a time based job scheduling service.
- `crontab`: a program to create, read, update, and delete your job schedules.
- `crontab file`: install a new crontab from file.
  - Create cron file: vi mycron >>
  - Add cron file to crontab: crontab mycron
- `crontab -l`: list your cron jobs.
- `crontab -e`: edit your cron jobs.
- `crontab -r`: remove all of your cron jobs.

Crontab Format:

```
* * * * * command
| | | | |
| | | | +-- Day of the Week (0-6)
| | | +---- Month of the Year (1-12)
| | +------ Day of the Month (1-31)
| +-------- Hour (0-23)
+---------- Minute (0-59)
```
- \# Run very Monday at 07:00 and send output.
  - `0 7 * * 1 /path > out.txt 2> err.txt`
- \# Run at 02:00 every day a python file.
  - `0 2 * * * /home/dgarrido/pyhello.py >> out.txt`
  ```
  #!/usr/bin/python3.6
  print("Hello World")
  ```
- \# Run every 30 minutes a bash script.
  - `0,30 * * * * /home/dgarrido/echohello.sh >> out.txt`
  ```
  #!/bin/sh
  echo "Hello World"
  ```
- \# Another way to do the same thing.
  - `*/2 * * * * /home/dgarrido/pyhello.sh >> outsh.txt`
  ```
  #!/bin/sh
  cd /home/dgarrido
  python3.6 pyhello.py
  ```
- \# Run for the first 5 minutes of the hour
  - `0-4 * * * * /home/dgarrido/Desktop/Development/Linux/dir/run.sh`
  ```
  #!/bin/sh
  cd ~/Desktop/MATERIAL_DOCENTE/7_septimo/Primavera/MIR/tareas/T1
  /home/dgarrido/anaconda3/envs/opencvenv/bin/python main.py 'television/mega-2014_04_17.mp4' 'comerciales/'
  ```

Shortcuts:
- `@yearly` = `0 0 1 1 *`
- `@annually` = `0 0 1 1 *`
- `@monthly` = `0 0 1 * *`
- `@weekly` = `0 0 * * 0`
- `@daily` = `0 0 * * *`
- `@midnight` = `0 0 * * *`
- `@hourly` = `0 * * * *`