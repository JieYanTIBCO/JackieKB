# Firewall Cheat Sheet

## Traditional Firewall Types

| Layer | Type | Target | Functions |
|-------|------|--------|-----------|
| L3/L4 (Network) | Packet Filtering Firewall | IP addresses, ports, protocols | - Basic packet filtering<br>- Access control lists<br>- Stateless packet inspection |
| L4/L5 (Transport) | Circuit-Level Gateway | Network connections, sessions | - Session monitoring<br>- State table maintenance<br>- Connection validation |
| L7 (Application) | Application Firewall | Application protocols, content | - Deep packet inspection<br>- Content filtering<br>- Application-specific rules |
| Multiple Layers | Next-Gen Firewall (NGFW) | All traffic aspects | - IPS/IDS integration<br>- SSL inspection<br>- User identity awareness |

## AWS Network Security Controls

### Comparison Table

| Control | Layer | Scope | Stateful | Key Features |
|---------|-------|-------|----------|--------------|
| Security Groups | Instance | Network interfaces | Yes | - Allow rules only<br>- Stateful<br>- Instance level<br>- Default: deny all |
| Network ACLs | Subnet | Subnet boundaries | No | - Allow/Deny rules<br>- Stateless<br>- Subnet level<br>- Rule number priority |
| AWS WAF | Application | ALB/CloudFront | N/A | - Web application firewall<br>- Custom rules<br>- Rate limiting |
| AWS Network Firewall | VPC | VPC level | Both | - Deep packet inspection<br>- Custom protocols<br>- Traffic filtering |

### Security Group vs NACL

| Feature | Security Group | Network ACL |
|---------|---------------|-------------|
| Scope | Instance level | Subnet level |
| Rule Types | Allow rules only | Allow and Deny rules |
| State | Stateful | Stateless |
| Rule Processing | All rules evaluated | Rules processed in order |
| Default Rule | Deny all | Deny all |
| Return Traffic | Automatically allowed | Must be explicitly allowed |

### Common Rule Examples

#### Security Groups

```plaintext
# Web Server Security Group
Inbound Rules:
- Allow TCP 80,443 from 0.0.0.0/0     # HTTP/HTTPS
- Allow TCP 22 from 10.0.0.0/8        # SSH from internal network

# Application Server Security Group
Inbound Rules:
- Allow TCP 8080 from Web-SG          # App traffic from web servers
- Allow TCP 22 from Bastion-SG        # SSH from bastion host

# Database Security Group
Inbound Rules:
- Allow TCP 3306 from App-SG          # MySQL from app servers
- Allow TCP 5432 from App-SG          # PostgreSQL from app servers
```

#### Network ACLs

```plaintext
# Public Subnet NACL
Inbound Rules:
100: Allow TCP 80,443 from 0.0.0.0/0      # Web traffic
200: Allow TCP 1024-65535 from 0.0.0.0/0  # Return traffic
* Deny all

Outbound Rules:
100: Allow TCP 80,443 to 0.0.0.0/0        # Web responses
200: Allow TCP 1024-65535 to 0.0.0.0/0    # Ephemeral ports
* Deny all

# Private Subnet NACL
Inbound Rules:
100: Allow TCP 3306 from 10.0.0.0/16      # Database traffic
200: Allow TCP 22 from 10.0.0.0/16        # Internal SSH
300: Allow TCP 1024-65535 from 0.0.0.0/0  # Return traffic
* Deny all
```

## OS-Level Firewalls

### Linux (iptables)

```bash
# Allow SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow established connections
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Default deny
iptables -P INPUT DROP
```

### Linux (ufw)

```bash
# Enable ufw
ufw enable

# Allow SSH
ufw allow 22/tcp

# Allow HTTP/HTTPS
ufw allow 80/tcp
ufw allow 443/tcp

# Allow from specific network
ufw allow from 192.168.1.0/24 to any port 3306
```

### Windows Firewall

```powershell
# Allow inbound port
New-NetFirewallRule -DisplayName "Allow RDP" -Direction Inbound -Protocol TCP -LocalPort 3389 -Action Allow

# Allow from specific IP
New-NetFirewallRule -DisplayName "Allow SSH from IP" -Direction Inbound -Protocol TCP -LocalPort 22 -RemoteAddress 192.168.1.0/24 -Action Allow
```

## Best Practices

### Security Groups

1. **Least Privilege**: Only allow required ports
2. **Source Restriction**: Use specific IPs/security groups instead of 0.0.0.0/0
3. **Tag Resources**: Use tags for easier management
4. **Regular Audit**: Review and remove unused rules
5. **Documentation**: Document purpose of each rule

### Network ACLs

1. **Rule Numbering**: Leave gaps between rule numbers (100, 200, 300)
2. **Ephemeral Ports**: Remember to allow return traffic (1024-65535)
3. **Backup**: Maintain backup of NACL rules
4. **Subnet Isolation**: Use different NACLs for public/private subnets
5. **Default Deny**: Keep default deny rule

### General

1. **Regular Updates**: Keep firewall software updated
2. **Monitoring**: Enable logging and alerts
3. **Documentation**: Maintain change log
4. **Testing**: Test rules before production
5. **Backup**: Regular backup of configurations

## Common Issues and Troubleshooting

### AWS Security Groups and NACLs

| Issue | Possible Causes | Troubleshooting Steps |
|-------|----------------|----------------------|
| Cannot SSH to EC2 | - Wrong security group rules<br>- Wrong key pair<br>- NACL blocking traffic | 1. Verify inbound rule for port 22<br>2. Check source IP/CIDR<br>3. Verify NACL allows port 22<br>4. Confirm correct key pair |
| Web Server Unreachable | - Security group missing HTTP/HTTPS<br>- NACL blocking traffic<br>- Route table issues | 1. Check ports 80/443 in security group<br>2. Verify NACL inbound/outbound rules<br>3. Check route table for internet gateway |
| Database Connection Failed | - Security group misconfiguration<br>- Wrong CIDR range<br>- Port mismatch | 1. Verify DB security group allows app server<br>2. Check port numbers (3306/5432)<br>3. Validate VPC CIDR ranges |
| Application Port Issues | - Missing rules for custom ports<br>- Wrong security group references<br>- Ephemeral ports blocked | 1. Add rules for application ports<br>2. Check security group references<br>3. Allow return traffic (1024-65535) |

### OS-Level Firewalls

| Issue | Possible Causes | Troubleshooting Steps |
|-------|----------------|----------------------|
| Service Unreachable | - Firewall blocking port<br>- Wrong rule order<br>- Default policy issues | 1. `sudo iptables -L -n -v`<br>2. Check rule ordering<br>3. Verify default policy |
| Connection Timeout | - Missing return traffic rules<br>- State tracking issues<br>- Wrong interface | 1. Add ESTABLISHED,RELATED rule<br>2. Check interface specifications<br>3. Verify bidirectional rules |
| Rules Not Working | - Rule precedence issues<br>- Interface mismatch<br>- Syntax errors | 1. Review rule order<br>2. Confirm correct interface<br>3. Check rule syntax |

### Diagnostic Commands

```bash
# AWS
aws ec2 describe-security-groups --group-id sg-xxxxx
aws ec2 describe-network-acls --network-acl-id acl-xxxxx

# Linux
iptables -L -n -v                    # List all rules with packet counts
netstat -tulpn                       # List listening ports
tcpdump -i eth0 port 80             # Monitor traffic on port 80

# Windows
Get-NetFirewallRule                  # List firewall rules
Test-NetConnection -Port 80          # Test port connectivity
netstat -ano | findstr :80           # Check port usage
```

### Common Resolution Steps

1. **Connectivity Issues**
   - Start with security group rules
   - Check NACL if security group looks correct
   - Verify route tables and network paths
   - Test with telnet or netcat

2. **Performance Problems**
   - Check rule order efficiency
   - Look for overlapping rules
   - Monitor packet drops
   - Review logging for blocked traffic

3. **Access Problems**
   - Verify CIDR ranges
   - Check protocol specifications
   - Confirm port numbers
   - Review rule priorities

4. **Rule Management**
   - Document all changes
   - Use descriptive names
   - Implement change control
   - Regular rule audits


~~--test--~~
