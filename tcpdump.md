# Using TCPDump with Specific Port Number, File Saving, and Reading

## Scenario: Capture Traffic for a Specific Port and Save to a File

To capture traffic with a specific port number using `tcpdump`, you can include the port number in your command. You can also save the captured packets to a `.pcap` file for later analysis and read that file using `tcpdump` as well.

### 1. Capture Traffic with a Specific Port Number and Save to a File

**Command:**

```bash
tcpdump -i any -n host 192.168.1.1 and port 443 -w capture.pcap

-i any: Captures traffic from all network interfaces.
-n: Prevents tcpdump from resolving hostnames, showing raw IP addresses.
host 192.168.1.1: Filters traffic to or from IP address 192.168.1.1.
and port 443: Filters traffic to capture only packets involving port 443 (HTTPS).
-w capture.pcap: Writes the captured traffic to a file called capture.pcap.

Extra Commands -
tcpdump -r capture.pcap
tcpdump -i any -n host 192.168.1.1 and \( port 443 or port 80 \) -w capture.pcap
tcpdump -i any -n src port 443 and host 192.168.1.1 -w capture.pcap
tcpdump -i any -n dst port 443 and host 192.168.1.1 -w capture.pcap
```
