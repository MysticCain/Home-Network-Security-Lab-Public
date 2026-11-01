# Validation and Troubleshooting Log

|Check|Method|Recorded result|Caveat|
|-|-|-|-|
|Trusted addressing|Windows `ipconfig`|Valid VLAN 10 IP and gateway|Snapshot of one client|
|Trusted-to-IoT reachability|Windows ping to IoT host|0% loss|ICMP only|
|IoT-to-Trusted reachability|Android ping utility to Trusted PC|100% loss|Host firewalls can influence results|
|Windows firewall cross-check|Inspected Private ICMP echo rule; temporarily enabled and retested|100% loss persisted|Other host/network filters not exhaustively ruled out|
|Firewall restoration|Disabled temporarily enabled Private ICMP echo rule|User confirmed completed|No broad firewall shutdown performed|
|AP management VLAN|Omada device status|AP assigned to VLAN 99|Confirms AP addressing, not all management ACLs|
|Wi-Fi association|`netsh wlan show interfaces`|5 GHz, channel 100, 802.11ac, 780 Mbps link|Link rate is not internet throughput|
|Internet performance|Ookla Speedtest|581.83 Mbps down, 491.22 Mbps up, 6 ms latency|One measurement|

## Useful diagnostic commands

```powershell
ipconfig /all
netsh wlan show interfaces
netsh wlan show networks mode=bssid
netsh advfirewall show currentprofile
netsh advfirewall firewall show rule name="File and Printer Sharing (Echo Request - ICMPv4-In)"
ping <IoT-device-IP>
```

## Follow-up validation plan

1. Use a known listening service on a Trusted test machine to verify blocked IoT-originated connections without relying solely on ICMP.
2. Test guest-to-internal and guest-to-internet access, recording both successful and failed expected outcomes.
3. Validate router management access restrictions separately from inter-VLAN forwarding ACLs.
4. Capture redacted screenshots of VLAN interfaces, wireless VLAN mappings, and ACL order.

