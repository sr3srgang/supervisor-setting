# supervisor-setting

## Linux
1. Copy `supervisord.conf.linux` as `supervisord.conf`.
2. Change the root folder path in the paths accross the lines in the file accordingly. e.g., keep `/etc/supervisor/` if the supervisor was installed via `sudo apt install supervisor` 
3. Make sure the necessary subfolders in the root folder:
    - `conf.d`
    - `logs`
    - `run`
    If they don't exists, create them or supervisor would not work; the below is the stdout of `sudo systemctl status --no-pager supervisor` when supervisor was installed as a systemd service deamon via `sudo apt install supervisor`:
    ```bash
    ● supervisor.service - Supervisor process control system for UNIX
        Loaded: loaded (/usr/lib/systemd/system/supervisor.service; enabled; preset: enabled)
        Active: activating (auto-restart) (Result: exit-code) since Mon 2025-03-10 15:40:12 MDT; 6s ago
        Docs: http://supervisord.org
        Process: 3156719 ExecStart=/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf (code=exited, status=2)
    Main PID: 3156719 (code=exited, status=2)
        Tasks: 21 (limit: 38053)
        Memory: 34.5M (peak: 230.7M)
            CPU: 112ms
        CGroup: /system.slice/supervisor.service
                └─3156573 python ./main.py
    ```