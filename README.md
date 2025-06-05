# NetworkMonitoringZeek
Network monitoring with Zeek with VM's

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Zeek-Based Network Traffic Monitoring and Analysis

📌 Project Title:

Network Traffic Monitoring Using Zeek and Manual Log Inspection


🎯 Objective:

This project focuses on implementing a passive network traffic monitoring system using Zeek (formerly Bro) and manually 
inspecting logs generated during network activity. It demonstrates how Zeek can identify and log connection attempts, detect 
unusual behavior, and generate actionable connection metadata.


🧰 Stack Used:

Zeek 7.2.1 — powerful network analysis tool

Ubuntu 24.04 — OS on both sensor and client VMs

Netcat — to simulate TCP traffic

VirtualBox — VM environment


🏗️ Architecture Overview:

sensor@network-monitoring (192.168.56.20) — runs Zeek and monitors network on interface enp0s8

client@network-monitoring (192.168.56.10) — generates test traffic to simulate real-world scenarios



🛠️ Setup Steps:

1. Assign Static IPs

Both VMs were configured with static IPs in /etc/netplan/01-netcfg.yaml. Network was restarted using:

sudo systemctl restart NetworkManager

2. Install Zeek on Sensor VM

Zeek was manually installed from the official openSUSE repository. Postfix was configured with Internet Site to satisfy mail dependencies. Zeek was added to the path in ~/.bashrc:

export PATH="/opt/zeek/bin:$PATH"

3. Create Log Directory

sudo mkdir -p /opt/zeek/logs
sudo chown $USER:$USER /opt/zeek/logs

4. Start Zeek Traffic Monitoring

Zeek was run in interface monitoring mode:

sudo zeek -i enp0s8

This captured real-time traffic to /opt/zeek/logs/<YYYY-MM-DD>/.


💣 Traffic Simulation

Performed on Client VM:

SSH Connection Attempt:

ssh sensor@192.168.56.20

Netcat TCP Simulation:

echo "Test Zeek capture" | nc 192.168.56.20 9999

These packets were captured and recorded by Zeek in its logs.


📂 Logs Analyzed:

Location: /opt/zeek/logs/<date>/

conn.log – All TCP/UDP/ICMP connections

dns.log – DNS queries

packet_filter.log – packet filtering info

weird.log – anomalous traffic

Sample Fields from conn.log

id.orig_h, id.resp_h: source and destination IPs

id.resp_p: destination port

proto: protocol used

conn_state: connection status (e.g., SF, S0, REJ, OTH)

Example Output:

192.168.56.10 → 192.168.56.20:22 (tcp) - REJ
192.168.56.10 → 192.168.56.20:9999 (tcp) - SF


✅ Outcome:

Successfully deployed Zeek for packet inspection on a virtualized network

Captured real traffic from client VM to sensor

Identified patterns and verified the packet metadata using conn.log

Practiced log correlation and interpretation without needing a visualization engine





-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

📘 Future Enhancements:

Automate log parsing with zeek-cut

Enable centralized logging with Filebeat + ELK

Apply custom Zeek scripts for attack detection (e.g., port scans, brute force)

Setup long-term storage for logs and rotate archives


🧠 Learning Benefits:

Understood how Zeek passively observes and logs traffic

Practiced using real-world tools in a controlled lab

Interpreted connection logs to infer network behavior

Gained hands-on skills in working with raw log data


📂 Artifacts:

Zeek logs (conn.log, dns.log, weird.log, etc.)

Packet capture (traffic.pcap)

Screenshots of successful test traffic and log output

Command history for configuration and execution steps
