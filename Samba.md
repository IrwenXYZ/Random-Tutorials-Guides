# Samba
Tutorials & guides to get Samba working.

## Set up a Samba share
1. Install Samba
   ```
   apt install samba
   ```
2. Configure a directory for Samba to share
   ```
   mkdir /path/to/directory
   ```
   
   Now set ownership to be yourself
   ```
   chown -R yourusername:yourusername /path/to/directory
   ```

   You'll need to set the permissions of the directory to `775`
   ```
   chmod 775 /path/to/directory
   ```
3. Configure Samba
   *This adds the directory as a Samba shared drive.*
   ```
   nano /etc/samba/smb.conf
   ```
   At the bottom of the file, add the following lines
   ```bash
   [nameofshare]
    comment = Brief description of share
    path = /path/to/share
    read only = no
    browsable = yes
   ```
4. Restart Samba
   ```
   systemctl restart smbd
   ```
5. Set user & password
   *Note that username used must belong to a system account, else it won't save.*
   ```
   smbpasswd -a username
   ```
6. Connect to the SMB share!