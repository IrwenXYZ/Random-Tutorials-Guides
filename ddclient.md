# ddclient
"Ddclient is a Perl client used to update dynamic DNS entries for accounts on 'Dynamic DNS Network Services' free DNS service."

Because the tutorials on ddclient online are mostly extemely convoluted and fairly outdated, I'm creating my own tutorial for this.

This version of the tutorial is verified to work with Cloudflare & ddclient 3.9.1.

1. Download ddclient 3.9.1 to your local machine.
   ```
   wget https://github.com/ddclient/ddclient/archive/refs/tags/v3.9.1.tar.gz
   ```
2. Extract the file & move into the extracted folder.
    ```
    tar xvfa v3.9.1.tar.gz
    cd ddclient-3.9.1
    ```
3. We now need to create a couple of directories that are missing.
    ```
    mkdir /etc/ddclient
    mkdir /var/cache/ddclient
    ```
4. Now, copy the default config file into the folder where your config file will reside.
    ```
    cp sample-etc_ddclient.conf /etc/ddclient/ddclient.conf
    ```
5. Go into the config file
    ```
    nano /etc/ddclient/ddclient.conf
    ```
6. Uncomment the following line by removing the `#` at the start of the line. This checks your public IP address by checking the specified website.
    ```conf
    use=web, web=checkip.dyndns.org/, web-skip='IP Address’
    ```
7. Now find the following in the config file and fill in your own details. I won't cover how to get these information.
    ```conf
    protocol=cloudflare,        \
    zone=$domain.tld,            \
    login=$your@cloudflare.login,     \
    password=$your_api_key             \
    ttl=1                       \
    $domain.tld,$subdomain1.domain.tld,$subdomain2.domain.tld
    ```
8. We will need to install a few modules for this to work properly.
    ```
    sudo apt install libdata-validate-ip-perl libjson-any-perl libio-socket-ssl-perl
    ```
9. Finally, we will copy the `ddclient` programme into the appropriate folder.
    ```
    cp ddclient /usr/sbin
    ```
10. Verify that everything works by running this command.
    ```
    sudo ddclient -daemon=0 -debug -verbose -noquiet
    ```
    If everything work, the last line should show something like this.
    ```
    subdomain1.domain.tld -- Updated Successfully to <your.ip.address.here>
    ```
11. Enable ddclient as a service.
    ```
    sudo cp sample-etc_systemd.service /lib/systemd/ddclient.service
    sudo systemctl daemon-reload
    sudo systemctl start ddclient
    sudo systemctl enable ddclent
    ```
    You can now see the status of ddclient through systemctl
    ```
    systemctl status ddclient
    ```
12. Now we can cleanup the files that were downloaded.
    ```
    cd
    sudo rm -r ddclient-3.9.1 v3.9.1.tar.gz
    ```