# unbound

## What is unbound?
Pi-Hole by default forwards any DNS requests to the DNS server that you indicated in the settings. This method opens up possibilities of DNS poisoning attacks. It has already happened, and we want to reduce the risk of being attacked as much as possible.

To achieve this, we need to set up a recursive DNS resolver, which is what unbound is.

## What then, is a recursive DNS resolver?
Honestly, read [this website](https://www.cloudflare.com/en-gb/learning/dns/dns-server-types/). Cloudflare can explain better than I can.

To add on, without unbound, Pi-Hole sends the query directly to an external DNS resolver like Cloudflare's 1.1.1.1, or Google's 8.8.8.8. These services communicate with the root, TLD, & authoritative servers on our behalf & caches it, which can make our internet traffic faster.

With unbound, we are taking the job of that DNS resolver. Our network will communicate directly with the root, TLD, & authoritative servers, which removes a middleman which can be a point of attack.

The main benefit of having our own DNS resolver is privacy. With our own resolver, no one server can fully log the exact paths that we're going, since each different website will need our server to query to root server, which bypasses the traditional DNS providers such as Cloudflare & Google.

However, because we have to traverse the entire path as described in Cloudflare's guide earlier, it can be a little slow when going to a site for the first time. However, subsequent visits will be much faster as it's already cached in our local unbound server, which will be configured for efficient caching.

*[Reference](https://docs.pi-hole.net/guides/dns/unbound/)*

## Installation
*Enough theory, time for the practical!*  
This guide is designed to be used together with a Pi-Hole installation.  
For extra configuration for speed & security, check out [anudeepND's unbound setup process](https://github.com/anudeepND/pihole-unbound).

1. Install unbound.
    ```
    apt install unbound
    ```
    When installing, the unbound DNS service might show to have failed to start. This is normal, since it's trying to run on port 53, but Pi-Hole is already using this port.
2. Create the configuration file.
    ```
    nano /etc/unbound/unbound.conf.d/pi-hole.conf
    ```
3. Copy the following into the new file.
    ```conf
    server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0

    interface: 127.0.0.1
    port: 5335 (Or whatever port you want to use)
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # May be set to yes if you have IPv6 connectivity
    do-ip6: no

    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no

    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"

    # Trust glue only if it is within the server's authority
    harden-glue: yes

    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes

    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no

    # Reduce EDNS reassembly buffer size.
    # Suggested by the unbound man page to reduce fragmentation reassembly problems
    edns-buffer-size: 1472

    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes

    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1

    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m

    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
    ```
4. Save the file, and restart unbound.
    ```
    systemctl restart unbound
    systemctl status unbound
    ```
    The unbound service should now be active and running.
5. Log in to the Pi-Hole admin page, and specify `127.0.0.1#5335` (or whatever port you used) as the Custom DNS (IPV4). Make sure that it's the only box that's checked. Remember to click on `Save`.

### Disable `resolvconf` for `unbound`
Unbound comes with a systemd service called `unbound-resolveconf.service`, which instructs `resolvconf` to write the DNS service for the local machine at `nameserver 127.0.0.1`, but without the 5335 port, into the file `/etc/resolv.conf`.

For our installation, because the service `unbound-resolveconf.service` doesn't work, we have to take this step to complete the installation. To check if it's working, run this command:
```
systemctl status unbound-resolveconf
```
If it's active, check if the IP address in `/etc/resolv.conf` matches the unbound IP address. Otherwise, skip ahead to the actual steps.
```
cat /etc/resolv.conf
```
If it isn't active, you'll have to follow the steps below.

1. Disable the service.
    ```
    systemctl disable unbound-resolvconf.service
    systemctl stop unbound-resolvconf.service
    ```
2. Configure our `domain_name_servers` in `/etc/dhcpcd.conf`.
    ```
    nano /etc/dhcpcd.conf
    ```
    Find the interface our machine is using, find and change the following line.
    ```conf
    static domain_name_servers=127.0.0.1
    ```
3. Restart `dhcpcd`.
    ```
    systemctl restart dhcpcd
    ```
4. Verify that the IP address matches the unbound IP address again.
    ```
    cat /etc/resolv.conf
    ```