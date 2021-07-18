# Ubuntu
Tutorials & guides related to the Ubuntu OS.

## Change Ubuntu Hostname
```bash
sudo hostname <new hostname>
sudo nano /etc/hostname # update with new hostname
sudo nano /etc/hosts # replace any old hostname with the new hostname
```
Reconnect after doing all steps.

## Change Locale & Timezone
1. Reconfigure the locales package & select `en_SG.UTF-8 UTF-8` (Since I stay in Singapore). Also set it as default locale for system environment.
    ```
    sudo dpkg-reconfigure locales
    ```
2. Set timezone using `timedatectl`.
    ```bash
    timedatectl list-timezones
    # the above is to check which timezone you should change to.
    sudo timedatectl set-timezone Asia/Singapore
    ```
## Unattended Upgrades
1. Install unattended-upgrades and other required tools.
    ```
    sudo apt install unattended-upgrades apt-listchanges
    ```
2. Reconfigure unattended-upgrades.
    ```
    sudo dpkg-reconfigure unattended-upgrades
    ```
3. Go to the config file `sudo nano /etc/apt/apt.conf.d/50unattended-upgrades`.

    Uncomment this line.
    ```
    Unattended-Upgrade::Remove-New-Unused-Dependencies "true";
    ```
4. Dry run the configuration.
    ```
    sudo unattended-upgrade --dry-run
    ```
5. Check the logs using this command:
    ```
    sudo cat /var/log/unattended-upgrades/unattended-upgrades.log
    ```

## SSH
### Changing SSH Ports
1. Enter the SSH configuration file.
    ```
    sudo nano /etc/ssh/sshd_config
    ```
2. Find the `Port` option
3. Change it to whatever port that will be used for SSH
4. Enable this setting by restarting SSH
    ```
    sudo systemctl restart ssh
    ```

### Disabling password authentication
**Make sure you have configured your public key in `.ssh` before doing this**
1. Enter the SSH configuration file.
    ```
    sudo nano /etc/ssh/sshd_config
    ```
2. Set the following settings & save the file.
    ```
    ChallengeResponseAuthentication no
    PasswordAuthentication no
    PermitRootLogin no
    PermitRootLogin prohibit-password
    ```
3. Restart SSH.
    ```
    sudo systemctl restart SSH
    ```
4. Edit the sudoers file to disable password authentication for `sudo`. Only do this if you're okay with running `sudo` commands without an extra layer of security.
    ```
    sudo visudo /etc/sudoers.d/90-cloud-init-users
    ```
5. Add the following line & save the file.
    ```
    <your_username> ALL=(ALL) NOPASSWD:ALL
    ```