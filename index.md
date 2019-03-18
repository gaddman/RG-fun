# Messing around with residential gateways
Poking around with some of Vodafone New Zealand's home gateways.

## Scripted data collection and reboot
For a Python script to collect some interesting info from your router, or to programmatically reboot it, check out https://github.com/gaddman/rgw-info. Supports UltraHub/UltraHub+ (Technicolor 500-T), Huawei HG659, Vodafone Station (Vox1.5 / Sercomm SHG1500), and Huawei B315.

## HG659
ADSL, VDSL, FTTH (UFB) and HFC (Cable/FibreX) router.

### Firmware
Testing with the [B026 firmware](http://downloads.vodafone.co.nz/HG659-16V100R001C206B026_main.bin)

Opening up the HG659 firmware to see what's inside.
Install [jefferson](https://github.com/sviehb/jefferson):
```bash
sudo apt-get install python-lzma python-pip
sudo pip install cstruct
git clone https://github.com/sviehb/jefferson.git
cd jefferson
```
And extract firmware:
```bash
$ jefferson -d HG659fs HG659-16V100R001C206B026_main.bin
dumping fs #1 to ./HG659fs/fs_1
Jffs2_raw_dirent count: 792
Jffs2_raw_inode count: 10188
Jffs2_raw_summary count: 0
Jffs2_raw_xattr count: 0
Jffs2_raw_xref count: 0
Endianness: Big
writing S_ISDIR bin
writing S_ISDIR config
writing S_ISDIR dev
writing S_ISDIR etc
writing S_ISDIR html
writing S_ISDIR lib
writing S_ISLNK linuxrc
writing S_ISDIR mnt
writing S_ISDIR proc
writing S_ISDIR sbin
writing S_ISDIR tmp
writing S_ISDIR usr
writing S_ISDIR var
<snip>
```

### TR 064 / UPnP
See TR064 scan at [HG659-TR064](HG659-TR064.md)

### Open ports
There's also port 1900/udp for UPnP not shown here:

```bash
$ nmap -p- 10.2.1.1
Starting Nmap 7.01 ( https://nmap.org ) at 2017-08-20 15:43 NZST
Nmap scan report for 10.2.1.1
Host is up (0.00071s latency).
Not shown: 65529 filtered ports
PORT      STATE SERVICE
22/tcp    open  ssh
53/tcp    open  domain
80/tcp    open  http
443/tcp   open  https
37215/tcp open  unknown
37443/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 148.98 seconds
```

## Ultra Hub
Vodafone Technicolor, model H 500-t

### TR 064 / UPnP
[Certified](https://openconnectivity.org/certified-product/vodafone-h-500-t) for media server and internet gateway.

No response to TR064 using the nmap script, but see full TR064 scan at [UltraHub-TR064](UltraHub-TR064.md)

```bash
$ sudo nmap -sU -p 1900 --script=upnp-info 192.168.1.1

Starting Nmap 7.01 ( https://nmap.org ) at 2017-09-07 17:34 NZST
Nmap scan report for ultrahub.hub (192.168.1.1)
Host is up (0.00036s latency).
PORT     STATE         SERVICE
1900/udp open|filtered upnp
MAC Address: 10:13:31:00:00:00 (Unknown)
```

### Open ports
```bash
$ nmap -p- 192.168.1.1

Starting Nmap 7.01 ( https://nmap.org ) at 2017-09-07 17:35 NZST
Nmap scan report for ultrahub.hub (192.168.1.1)
Host is up (0.00072s latency).
Not shown: 65527 closed ports
PORT      STATE    SERVICE
22/tcp    filtered ssh
53/tcp    open     domain
80/tcp    open     http
443/tcp   open     https
631/tcp   filtered ipp
8080/tcp  open     http-proxy
49152/tcp open     unknown
51005/tcp filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 2626.50 seconds
```
