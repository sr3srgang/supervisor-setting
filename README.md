# supervisor-setting

## Linux
1. Download `linux/supervisord.conf`.
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

```bash
conda create -n supervisor python=3.11 supervisor
```

As of 2025/04/07 supervisor-4.2.5 is installed.

Download/create/copy the files and folders in `./etc/` folder in the system's `/etc/` folder accordingly.
cf. the `.service` files for default, system-wide services (including `supervisord` installed by `apt`) are in `/lib/systemd/system/` and the custom services in `/etc/systemd/system/` override the system-wide services.

Open `supervisor.service` and replace the `<path to conda env>` placeholders with the actual path that can be found from `conda env list`.

Make symlink for `supervisorctl` in conda to the local `PATH` (to run `supervisorctl` just by typing so):
```bash
sudo ln -s /home/username/miniconda3/envs/myenv/bin/supervisorctl /usr/local/bin/supervisorctl
```

Change group to `users` and give read, write & file create group permissions of `/etc/supervisor/ and its subfile/folders to edit with `users` (e.g., through SSH or vscode tunnel). 
```bash
sudo mkdir /etc/supervisor
sudo chown -R :users /etc/supervisor
sudo chmod -R g+rwx /etc/supervisor
```
Verify the change
```bash
<host>:/etc$ ls -l /etc | grep supervisor
drwxrwxr--  2 root                 users                 4096 Apr  7 16:57 supervisor
```

Then uncomment the below lines to connect the supervisor to multivisor
```bash
[rpcinterface:multivisor]
supervisor.rpcinterface_factory = multivisor.rpc:make_rpc_interface
bind=*:9002
```

Use `/workspaces/supervisor-setting/linux/etc/supervisor/conf.d/template.conf.template` to create `.conf` files in the same folder for programs managed by supervisor.



## Windows
place the `supervisor` folder in `C:\supervisor`