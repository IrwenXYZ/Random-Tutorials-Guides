# Random-Tutorials-Guides

Hello! This repository holds the collection of tutorials & guides that I've created in my journey of creating & maintaining a home server. This can range from Linux stuff to self-hosting services for my own use.

This repository *will* contain things that might seem very simple, and *definitely will not* be as organised as it could be, but as a self taught server administrator, I had to Google my way out of every issue I had, and this is my way of consolidating all the information I've learnt.

I use Ubuntu Server 20.04 LTS, so all tutorials will be based on this.

## Contents
### Ubuntu Stuff
This contains the tutorials & guides related to the OS itself.
- [Change Ubuntu Hostname](Ubuntu.md/#change-ubuntu-hostname)
- [Change Locale & Timezone](Ubuntu.md/$change-locale--timezone)
- [Unattended Upgrades](Ubuntu.md/#unattended-upgrades)
- [Change SSH Port](Ubuntu.md/#change-ssh-port)
- [Disable password authentication](Ubuntu.md/#disable-password-authentication)
- [Check logs of a particular service](Ubuntu.md/#check-logs-of-a-particular-service)

### ddclient
Because the tutorials on ddclient online are mostly extemely convoluted and fairly outdated, this is my own guide.

[*Link*](ddclient.md)

### Pi-hole, PiVPN, & Unbound (PPU) stack
This started off with a desire to want to block ads & malware from my home network.

That's where Pi-hole came in.

And eventually I realised that I wanted something more than uBlock on my browser, and wanted ad blocking coverage across all apps. Without root, it's pretty hard to do this, but using apps that connect to different DNS servers seems sketchy, so I decided to use my own Pi-hole at home. Using this method, I can also remote into my server at home at any time.

That's where PiVPN came in.

One day, I was bored, and came across unbound. It seemed like a pretty cool concept, and in the long term, could even speed up loading speeds in my home network. At the same time, I could have more privacy, while reducing dependency on a DNS provider.

That's where unbound came in.

These three form what I call the PPU stack, where it provides security, speed, & privacy all in one, self hosted.

[Pi-hole](Pi-hole,%20PiVPN,%20&%20Unbound%20stack/Pi-hole.md)  
[PiVPN](Pi-hole,%20PiVPN,%20&%20Unbound%20stack/PiVPN.md)  
[Unbound](Pi-hole,%20PiVPN,%20&%20Unbound%20stack/unbound.md)