# Messing around with residential gateways
Poking around with some of Vodafone New Zealand's home gateways.

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

### TR 064 / UPNP
See TR064 scan at [HG659-TR064](HG659-TR064.md)

## Open ports
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
