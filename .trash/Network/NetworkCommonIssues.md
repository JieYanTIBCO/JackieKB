# Network Issues Reference Guide

## Table of Contents

- [Quick Reference](#quick-reference)
- [Issue Categories](#issue-categories)
  - [Category A: OS/Software/Configuration Issues](#category-a-ossoftwareconfiguration-issues)
  - [Category B: Infrastructure/Physical Issues](#category-b-infrastructure-physical-issues)
- [Network Issue Categories Diagram](#network-issue-categories-diagram)

## Quick Reference

| Severity | Response Time | Issue Types |
|----------|--------------|-------------|
| 🔴 Critical | < 30 mins | Complete outages, Security breaches |
| 🟡 High | < 2 hours | Performance degradation, Limited access |
| 🟢 Medium | < 4 hours | Non-critical services affected |
| ⚪ Low | < 8 hours | Minor issues, Workarounds available |

## Issue Categories

### Category A: OS/Software/Configuration Issues

| #   | Scenario                      | Symptoms                               | Common Root Causes                                                                                            | Troubleshooting Steps                                                                                                     | Key Tools/Commands                      |
| --- | ----------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 1   | 🔴 DNS Resolution Problems    | Cannot resolve hostnames               | • DNS server outage<br>• Incorrect DNS settings<br>• Corrupted DNS cache<br>• DNS record issues               | 1. Test with IP directly<br>2. Check DNS settings<br>3. Flush DNS cache<br>4. Try alternate DNS                           | `nslookup`, `dig`, `ipconfig /flushdns` |
| 2   | 🟡 DHCP Not Assigning IPs     | "No IP configuration"                  | • DHCP server down<br>• IP pool exhaustion<br>• DHCP scope issues<br>• Client configuration                   | 1. Try static IP temporarily<br>2. Restart DHCP service<br>3. Check DHCP logs<br>4. Verify IP pool                        | `ipconfig /release`, `ipconfig /renew`  |
| 3   | 🔴 VPN Connection Failures    | Cannot establish tunnel                | • Authentication issues<br>• VPN service down<br>• Connection blocking<br>• Policy mismatches                 | 1. Check credentials<br>2. Verify server reachable<br>3. Check client logs<br>4. Review VPN configs                       | VPN client logs, `ping`, `traceroute`   |
| 4   | 🟡 Firewall Blocking Traffic  | Specific services unreachable          | • Restrictive policies<br>• Default deny rules<br>• Stateful inspection issues<br>• Application filtering     | 1. Temporarily disable firewall<br>2. Check rule configurations<br>3. Test with specific ports<br>4. Review firewall logs | `telnet`, `nc`, firewall logs           |
| 5   | 🟡 SSL/TLS Certificate Errors | Browser security warnings              | • Expired certificates<br>• Hostname mismatch<br>• Untrusted CA<br>• Revoked certificates                     | 1. Verify certificate validity<br>2. Check certificate chain<br>3. Confirm correct hostname<br>4. Review expiration date  | `openssl`, browser developer tools      |
| 6   | 🟢 Application Timeouts       | Services start but fail                | • Resource exhaustion<br>• Backend latency<br>• Connection limits<br>• Improper timeout values                | 1. Check service status<br>2. Verify connectivity to backends<br>3. Review timeout settings<br>4. Monitor resource usage  | `netstat`, `curl`, application logs     |
| 7   | 🟡 Load Balancer Issues       | Uneven distribution                    | • Health check failures<br>• Backend server issues<br>• Sticky session problems<br>• Algorithm misconfigs     | 1. Check health probe status<br>2. Verify backend health<br>3. Review LB configuration<br>4. Monitor traffic patterns     | LB logs, `curl`, backend metrics        |
| 8   | 🔴 AWS VPC Connectivity       | EC2 instances unreachable              | • Security group rules<br>• Route table misconfigs<br>• NACL blocking<br>• IGW/NAT issues                     | 1. Check security groups<br>2. Verify route tables<br>3. Test IGW/NAT gateway<br>4. Review NACLs                          | VPC Flow Logs, Reachability Analyzer    |
| 9   | 🔴 NAT Gateway Problems       | Private instances can't reach internet | • NAT failure<br>• Route table issues<br>• Security group rules<br>• EIP problems                             | 1. Check NAT status<br>2. Verify route tables<br>3. Test outbound rules<br>4. Confirm elastic IP                          | AWS console, VPC Flow Logs              |
| 10  | 🟢 Port Forwarding Failures   | External access blocked                | • Router misconfig<br>• Double-NAT issues<br>• Firewall blocking<br>• ISP restrictions                        | 1. Verify router configuration<br>2. Check firewall rules<br>3. Test locally first<br>4. Confirm public IP                | `nc`, port scanning tools               |
| 11  | 🟡 BGP Peering Problems       | Routes not advertised                  | • ASN mismatches<br>• Auth failures<br>• Route policies blocking<br>• Peer timeouts                           | 1. Verify BGP neighbor status<br>2. Check AS numbers<br>3. Review route policies<br>4. Confirm authentication             | `show ip bgp neighbors`, router logs    |
| 12  | ⚪ DNS Propagation Delay       | Recent changes not visible             | • High TTL values<br>• Caching at multiple levels<br>• DNS server replication<br>• Anycast routing            | 1. Check TTL values<br>2. Verify with multiple resolvers<br>3. Force cache flush<br>4. Wait for propagation               | `dig +trace`, DNS lookup tools          |
| 13  | 🟢 QoS Configuration Issues   | Prioritized traffic delayed            | • Mismarked traffic<br>• Queue config errors<br>• Bandwidth allocation<br>• Policy mismatches                 | 1. Check QoS policies<br>2. Test traffic marking<br>3. Review queuing configuration<br>4. Monitor priority queues         | QoS monitoring, packet analyzers        |
| 14  | 🟡 Proxy Server Problems      | Web access selective failures          | • Proxy overload<br>• Configuration errors<br>• Cache corruption<br>• Authentication issues                   | 1. Bypass proxy temporarily<br>2. Check proxy logs<br>3. Review proxy settings<br>4. Test direct connection               | Proxy logs, browser network tools       |
| 15  | 🔴 DNS Rebinding              | Security blocks                        | • Security policy blocks<br>• Same-origin policy<br>• Reverse DNS mismatches<br>• Hostname validation         | 1. Check DNS security policies<br>2. Review browser security<br>3. Check local hosts file<br>4. Update security software  | DNS logs, security settings             |
| 16  | 🟡 Transit Gateway Problems   | Cross-VPC access failures              | • TGW attachment issues<br>• Route propagation<br>• Multiple route tables<br>• Security group incompatibility | 1. Check TGW route tables<br>2. Verify attachments<br>3. Review security groups<br>4. Test direct peering                 | TGW Network Manager, VPC Flow Logs      |

### Category B: Infrastructure/Physical Issues

| # | Scenario | Symptoms | Common Root Causes | Troubleshooting Steps | Key Tools/Commands |
|---|----------|----------|-------------------|---------------------|-------------------|
| 1 | 🔴 Basic Connectivity Failure | Cannot reach resources | • Cable/hardware failures<br>• IP misconfiguration<br>• Default gateway issues<br>• Network interface down | 1. Check physical connections<br>2. Verify IP configuration<br>3. Test local network<br>4. Check gateway | `ping`, `ipconfig/ifconfig`, `route print` |
| 2 | 🟡 Slow Network Performance | High latency, timeouts | • Network congestion<br>• Bandwidth limitations<br>• Hardware bottlenecks<br>• QoS issues | 1. Check bandwidth usage<br>2. Run speed test<br>3. Test with minimal traffic<br>4. Check for packet loss | `speedtest-cli`, `iperf`, `mtr` |
| 3 | 🟡 Intermittent Connection | Random disconnects | • Interference<br>• Hardware instability<br>• Driver issues<br>• Power fluctuations | 1. Monitor connection over time<br>2. Check for interference<br>3. Review logs<br>4. Replace cables/equipment | `ping -t`, network monitoring tools |
| 4 | 🟢 Wi-Fi Connection Issues | Poor wireless signal | • Interference<br>• Distance limitations<br>• Channel congestion<br>• Outdated firmware | 1. Check signal strength<br>2. Change Wi-Fi channel<br>3. Relocate access point<br>4. Update firmware | Wi-Fi analyzer, router admin console |
| 5 | 🟡 Packet Loss | Degraded performance | • Congestion<br>• Hardware errors<br>• Buffer overflows<br>• Misconfigurations | 1. Check route quality<br>2. Identify bottlenecks<br>3. Test different paths<br>4. Check for physical issues | `mtr`, `pathping`, `tcpping` |
| 6 | 🟢 IP Address Conflicts | Duplicate IP detected | • Static IP misconfiguration<br>• DHCP overlap<br>• Rogue DHCP servers<br>• Subnet mask issues | 1. Identify conflicting devices<br>2. Check DHCP reservations<br>3. Use static IPs<br>4. Segment network | `arp -a`, network scanner |
| 7 | 🟢 High Bandwidth Usage | Network congestion | • Video streaming<br>• Backup processes<br>• Malware/unwanted traffic<br>• P2P applications | 1. Identify top talkers<br>2. Monitor traffic patterns<br>3. Implement QoS<br>4. Segment traffic | NetFlow, packet analyzers |
| 8 | 🔴 Routing Loops | Extremely high latency | • Bad route redistribution<br>• Missing null routes<br>• Duplicate subnets<br>• Misconfigured OSPF/BGP | 1. Trace packet path<br>2. Check routing tables<br>3. Review router configs<br>4. Test with static routes | `traceroute`, router logs |
| 9 | 🔴 Spanning Tree Issues | Network instability | • Physical loops<br>• STP misconfig<br>• Bridge priority issues<br>• BPDU problems | 1. Check for loops<br>2. Verify STP configuration<br>3. Review switch logs<br>4. Trace cable paths | Switch logs, topology maps |
| 10 | 🟡 MTU Fragmentation | Large packets dropped | • Path MTU discovery fails<br>• VPN/tunnel overhead<br>• Jumbo frames issues<br>• Device misconfigs | 1. Test with varied packet sizes<br>2. Check MTU settings<br>3. Adjust MTU on devices<br>4. Test path MTU discovery | `ping -f -l size`, MTU testing tools |
| 11 | 🔴 Direct Connect Issues | AWS private link down | • Circuit problems<br>• Provider issues<br>• BGP misconfig<br>• VLAN tagging errors | 1. Check BGP status<br>2. Verify physical cross-connects<br>3. Confirm VLAN tagging<br>4. Test with public connections | AWS DC logs, BGP neighbor status |
| 12 | 🟢 IPv6 Connectivity Issues | Dual-stack failures | • IPv6 not supported<br>• Router advertisement issues<br>• DNS AAAA missing<br>• Application compatibility | 1. Test IPv4 only<br>2. Check IPv6 configuration<br>3. Verify router advertisements<br>4. Check DNS AAAA records | `ping6`, `tracert6`, `ipconfig` |
| 13 | 🟡 Multicast Routing Failures | Streaming/video issues | • PIM configuration<br>• IGMP filtering<br>• Multicast scope<br>• Layer 2 issues | 1. Verify IGMP configuration<br>2. Check multicast routing<br>3. Test with unicast<br>4. Review PIM settings | `ping -t 224.x.x.x`, multicast tools |
| 14 | 🔴 Network Equipment Failure | Complete outage | • Hardware failure<br>• Power issues<br>• Software crashes<br>• Configuration corruption | 1. Check power/connectivity<br>2. Review system logs<br>3. Test redundant paths<br>4. Replace hardware | SNMP monitoring, equipment diagnostics |

## Network Issue Categories Diagram

```mermaid
flowchart TD
    classDef critical fill:#ff4d4d,stroke:#ff0000,color:#fff
    classDef high fill:#ffd700,stroke:#ff8c00,color:#000
    classDef medium fill:#90EE90,stroke:#32CD32,color:#000
    classDef low fill:#e6e6e6,stroke:#808080,color:#000

    Start([Network Issue Detected]) --> Decision{Issue Type?}
    
    Decision -->|OS/Software/Config| ConfigIssues
    Decision -->|Infrastructure/Physical| InfraIssues
    
    subgraph ConfigIssues["Category A: OS/Software/Config Issues"]
        direction TB
        A1["DNS Resolution Problems"]:::critical
        A2["DHCP Issues"]:::high
        A3["VPN Failures"]:::critical
        A4["Firewall Problems"]:::high
        A5["Certificate Errors"]:::high
        A6["App Timeouts"]:::medium
        A7["Load Balancer"]:::high
        A8["AWS VPC"]:::critical
    end
    
    subgraph InfraIssues["Category B: Infrastructure/Physical Issues"]
        direction TB
        B1["Basic Connectivity"]:::critical
        B2["Network Performance"]:::high
        B3["Connection Stability"]:::high
        B4["Physical Layer"]:::critical
        B5["Equipment Issues"]:::critical
        B6["Routing Problems"]:::high
    end
    
    ConfigIssues --> ConfigTools["Tools:
    - DNS: nslookup, dig
    - Network: netstat, curl
    - Security: openssl
    - Cloud: AWS console"]
    
    InfraIssues --> InfraTools["Tools:
    - Basic: ping, traceroute
    - Performance: iperf, mtr
    - Analysis: Wireshark
    - Monitoring: SNMP"]
```

> Note: This guide provides a structured approach to diagnosing and resolving common network issues. The severity indicators (🔴 Critical, 🟡 High, 🟢 Medium, ⚪ Low) help prioritize response times and resource allocation.
