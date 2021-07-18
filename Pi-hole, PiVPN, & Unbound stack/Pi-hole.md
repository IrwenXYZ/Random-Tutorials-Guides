# Pi-Hole
Pi-hole is a black hole for ads, tracking & analytics, malware, and more.
1. Download & run the one-step automated installer.
    ```
    curl -sSL https://install.pi-hole.net | bash
    ```
2. Go through the installation steps, selecting the appropriate settings.
4. Set a password for the admin panel.
    ```
    pihole -a -p
    ```
5. After installation, go to the admin panel at `http://machine.ip.address/admin` and log in.
6. Install unbound (skip if not using unbound).
7. Enable Pi-Hole's DHCP server (under Admin -> DHCP), & disable the router's DHCP server.
8. Restart all devices.