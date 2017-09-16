# TR064 info from Vodafone UltraHub
Also known as the Vodafone H-500-t or Technicolor Vodafone-DGA0130VDF-NZ

An nmap scan with the [upnp-info](https://nmap.org/nsedoc/scripts/upnp-info.html) script doesn't provide a response:
```bash
$ sudo nmap -sU -p 1900 --script=upnp-info 10.2.1.1

Starting Nmap 7.01 ( https://nmap.org ) at 2017-09-07 17:34 NZST
Nmap scan report for ultrahub.hub (10.2.1.1)
Host is up (0.00036s latency).
PORT     STATE         SERVICE
1900/udp open|filtered upnp
MAC Address: 10:13:31:[redacted] (Unknown)
```

But using [upnp_info.py](https://github.com/gaddman/upnp_info) we see more. The default settings (after a factory reset) have WPS on and UPnP IGD off:
```bash
$ python upnp_info.py
[+] Discovering UPnP locations
[+] Discovery complete
[+] 2 locations found:
        -> http://10.2.1.6:49152/wps_device.xml
        -> http://10.2.1.1:49152/gax5ilPYp1/wps_device.xml
[+] Loading http://10.2.1.6:49152/wps_device.xml...
        -> Server String: Unspecified, UPnP/1.0, Unspecified
        ==== XML Attributes ===
        -> Device Type: urn:schemas-wifialliance-org:device:WFADevice:1
        -> Friendly Name: Vodafone-DGA0130VDF-NZ
        -> Manufacturer: Technicolor
        -> Model Name: MediaAccess
        -> Model Number: Vodafone-DGA0130VDF-NZ
        -> Serial Number: [redacted]
        -> Services:
                => Service Type: urn:schemas-wifialliance-org:service:WFAWLANConfig:1
                => Control: wps_control
                => Events: wps_event
                => API: http://10.2.1.6:49152/wps_scpd.xml
                        - GetDeviceInfo
                        - PutMessage
                        - PutWLANResponse
                        - SetSelectedRegistrar
        [+] M1 available. Looking up device information...
                MAC Address: 12:13:31:[redacted]
                Nonce: Tt6kRucmABF2h+kLiPLkzA==
                Public Key: [redacted]
                Manufacturer: Technicolor
                Model Name: MediaAccess
                Model Number: Vodafone-DGA0130VDF-NZ
                Serial Number: [redacted]
                Device Name: Vodafone-DGA0130VDF-NZ
[+] Loading http://10.2.1.1:49152/gax5ilPYp1/wps_device.xml...
        -> Server String: Unspecified, UPnP/1.0, Unspecified
        ==== XML Attributes ===
        -> Device Type: urn:schemas-wifialliance-org:device:WFADevice:1
        -> Friendly Name: Vodafone-DGA0130VDF-NZ
        -> Manufacturer: Technicolor
        -> Model Name: MediaAccess
        -> Model Number: Vodafone-DGA0130VDF-NZ
        -> Serial Number: [redacted]
        -> Services:
                => Service Type: urn:schemas-wifialliance-org:service:WFAWLANConfig:1
                => Control: wps_control
                => Events: wps_event
                => API: http://10.2.1.1:49152/wps_scpd.xml
[!] Could not load http://10.2.1.1:49152/wps_scpd.xml
[+] Fin.
```

Strangely it responds on the main IP address (10.2.1.1 here) as well as a secondary IP address (10.2.1.6 above). This secondary IP seems to vary between devices. The primary IP includes an extra random set of letters in the path, again different on different devices.

Turning off WPS, and enabling IGD UPnP in the advanced options shows the IGD response:
```bash
$ python upnp_info.py
[+] Discovering UPnP locations
[+] Discovery complete
[+] 1 locations found:
        -> http://10.2.1.1:5000/rootDesc.xml
[+] Loading http://10.2.1.1:5000/rootDesc.xml...
        -> Server String: OpenWRT/OpenWrt/Attitude_Adjustment__r43446_ UPnP/1.1 MiniUPnPd/1.8
        ==== XML Attributes ===
        -> Device Type: urn:schemas-upnp-org:device:InternetGatewayDevice:1
        -> Friendly Name: Vodafone-DGA0130VDF-NZ
        -> Manufacturer: Technicolor
        -> Manufacturer URL: http://www.technicolor.com/
        -> Model Description: Technicolor Internet Gateway Device
        -> Model Name: MediaAccess
        -> Model Number: Vodafone-DGA0130VDF-NZ
        -> Serial Number: [redacted]
        -> Services:
                => Service Type: urn:schemas-upnp-org:service:Layer3Forwarding:1
                => Control: /rifnBsj/ctl/L3F
                => Events: /rifnBsj/evt/L3F
                => API: http://10.2.1.1:5000/rifnBsj/L3F.xml
                        - SetDefaultConnectionService
                        - GetDefaultConnectionService
                => Service Type: urn:schemas-upnp-org:service:WANCommonInterfaceConfig:1
                => Control: /rifnBsj/ctl/CmnIfCfg
                => Events: /rifnBsj/evt/CmnIfCfg
                => API: http://10.2.1.1:5000/rifnBsj/WANCfg.xml
                        - GetCommonLinkProperties
                        - GetTotalBytesSent
                        - GetTotalBytesReceived
                        - GetTotalPacketsSent
                        - GetTotalPacketsReceived
                => Service Type: urn:schemas-upnp-org:service:WANIPConnection:1
                => Control: /rifnBsj/ctl/IPConn
                => Events: /rifnBsj/evt/IPConn
                => API: http://10.2.1.1:5000/rifnBsj/WANIPCn.xml
                        - SetConnectionType
                        - GetConnectionTypeInfo
                        - RequestConnection
                        - ForceTermination
                        - GetStatusInfo
                        - GetNATRSIPStatus
                        - GetGenericPortMappingEntry
                        - GetSpecificPortMappingEntry
                        - AddPortMapping
                        - DeletePortMapping
                        - GetExternalIPAddress
        [+] IGD port mapping available. Looking up current mappings...
[+] Fin.
```
