# networkwalks-B084C-Week1-Cybersecurity-lab-setup
Cybersecurity Lab Setup
# Kali Linux Pentest Lab in VirtualBox

*This project is part of my ongoing beginner cybersecurity and SOC analyst learning journey. All practical activities are conducted in my own lab or other authorized environments.*

A self-contained virtual lab I built for learning ethical hacking and defensive security. Everything runs on my own computer, so I can practise without risk to real systems.

## Summary

I built an isolated network in VirtualBox and placed a Kali Linux machine on it with a fixed IP address, internet access and file sharing with my host. This lab is the base for the Linux, networking and security exercises I plan to do next.

## Lab at a glance

| Item | Detail |
|------|--------|
| Hypervisor | Oracle VirtualBox 7.2.22 |
| Guest OS | Kali Linux 2026.2 |
| Host | Windows 10, 16 GB RAM, 500 GB SSD |
| Network | NAT Network, `10.0.0.0/24` |
| Kali address | `10.0.0.2/24` |
| Gateway | `10.0.0.1` |
| DNS | `8.8.8.8` |
| Snapshot | "My First Project" |

## How I built it

**1. Prepared the host**
I installed 7-Zip to unpack the Kali image, then installed VirtualBox. My computer needed no BIOS changes to run virtual machines.

**2. Created the isolated network**
In VirtualBox I went to *File → Tools → Network → NAT Networks* and created `NatNetwork` with the prefix `10.0.0.0/24` and DHCP on. Every VM I add later will join this same network.

**3. Brought in Kali**
I downloaded the prebuilt Kali VirtualBox image, extracted it and imported it. In the VM settings I:
- attached the network adapter to `NatNetwork`
- set clipboard and drag and drop to bidirectional
- shared my host Downloads folder

**4. Gave Kali a fixed address**
Under *Edit Connections → IPv4 Settings* I switched to Manual and entered `10.0.0.2`, netmask `24`, gateway `10.0.0.1` and DNS `8.8.8.8`.

**5. Saved a restore point**
Once everything was set up, I took a snapshot named "My First Project". If I break something during testing, I can return to this clean state.

## Checking the setup

```bash
ip addr                  # confirm 10.0.0.2/24 is assigned
nmcli device status      # confirm the interface is connected
ping -c 4 8.8.8.8        # raw internet connectivity
ping -c 4 google.com     # name resolution
```

## Troubleshooting reference

I didn't run into internet problems. For anyone who does: Kali can lose connectivity on VirtualBox 7 with Kali 2026.1 or newer. The fix is to turn off the duplicate-address detection delay on the connection and restart it:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

Other things to check first: that the NAT network was created correctly, that the adapter settings are right, and that no other VM is using `10.0.0.2`.

## What this taught me

- A NAT network keeps lab machines separated from my home network while still giving them internet.
- A static IP makes lab targets predictable and easy to reference.
- A ping to an IP address and a ping to a domain name test different things, so comparing them shows whether a problem is routing or DNS.
- NetworkManager can be controlled entirely from the terminal with `nmcli`.
- Snapshots make experimenting safe.

## Screenshots
<img width="960" height="540" alt="NUMBER4" src="https://github.com/user-attachments/assets/8f7ac2f0-047f-479d-ae77-95b52af4f17a" />
<img width="960" height="540" alt="NUMBER 3" src="https://github.com/user-attachments/assets/ddb1a30c-5063-4932-a489-d8de1f010f6f" />
<img width="587" height="388" alt="NUMBER 2" src="https://github.com/user-attachments/assets/c073241c-663d-4d8d-a020-f6d8e6724c94" />
<img width="960" height="540" alt="NAT" src="https://github.com/user-attachments/assets/ffbcf57f-82ed-41fb-82ab-0e00340fd6d3" />

The `screenshots/` folder contains images of:
- the NAT Network settings
- the Kali VM network adapter
- the IPv4 configuration
- the terminal output from the connectivity tests

## Planned next

- Add Windows and Android machines to the same network and test connectivity between them
- Practise core Linux commands
- Capture and read traffic with Wireshark
- Move on to basic log analysis
