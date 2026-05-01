# 📁 FTP (File Transfer Protocol) – Complete Guide for Cybersecurity

## 📌 Table of Contents

1. What is FTP?
2. How FTP Works (Active vs Passive Mode)
3. FTP vs SFTP vs FTPS
4. Basic FTP Commands
5. FTP Response Codes
6. Common FTP Vulnerabilities
7. FTP Security Testing (Nmap, Hydra, Metasploit)


---

## 1. What is FTP?

FTP (File Transfer Protocol) is a standard network protocol used to transfer files between a client and a server on a computer network.

- **Port 21** – Control Connection (commands, authentication)
- **Port 20** – Data Connection (actual file transfer)
- **Authentication:** Username and password (sent in plain text)
- **Stateless:** Each command is independent

How it works:
Client --(Command: LIST, GET, PUT)--> Server (Port 21)
Client <--(File data / Directory listing)-- Server (Port 20)

---

## 2. Active vs Passive Mode

| Mode | Data Connection Initiated By | Firewall Friendly |
|------|------------------------------|-------------------|
| Active | Server -> Client | No |
| Passive | Client -> Server | Yes |

**Active Mode:** Client sends PORT command with its IP and port. Server initiates data connection back to client on Port 20. Often blocked by firewalls.

**Passive Mode:** Client sends PASV command. Server opens a random high port and sends it to client. Client initiates data connection to that port. Works better with firewalls.

---

## 3. FTP vs SFTP vs FTPS

| Protocol | Port | Encryption | Authentication | Best For |
|----------|------|------------|----------------|----------|
| FTP | 21, 20 | No | Plain text | Legacy systems only |
| FTPS | 990, 989 | Yes (SSL/TLS) | Certificate + password | When FTP is required but encryption is needed |
| SFTP | 22 | Yes (SSH) | SSH keys or password | Recommended for modern use |

> Never use plain FTP over the internet. Use SFTP or FTPS instead.

---

## 4. Basic FTP Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `open` | Connect to FTP server | `open ftp.example.com` |
| `user` | Send username | `user admin` |
| `pass` | Send password | `pass password123` |
| `ls` / `dir` | List files | `ls` |
| `cd` | Change directory | `cd /public` |
| `get` | Download a file | `get file.txt` |
| `put` | Upload a file | `put localfile.txt` |
| `mget` | Download multiple files | `mget *.txt` |
| `mput` | Upload multiple files | `mput *.jpg` |
| `delete` | Delete a file | `delete file.txt` |
| `mkdir` | Create directory | `mkdir newfolder` |
| `rmdir` | Remove directory | `rmdir oldfolder` |
| `binary` | Binary transfer mode (images, exe, zip) | `binary` |
| `ascii` | ASCII transfer mode (text files) | `ascii` |
| `bye` / `quit` | Exit FTP | `bye` |


---

## 5. FTP Response Codes

| Code | Meaning | Security Relevance |
|------|---------|---------------------|
| 110 | Restart marker reply | Informational |
| 120 | Service ready in nnn minutes | Informational |
| 125 | Data connection already open | Informational |
| 150 | File status okay | Informational |
| 200 | Command okay | Success |
| 214 | Help message | Informational |
| 220 | Service ready for new user | FTP service is running |
| 221 | Service closing control connection | Normal logout |
| 226 | Closing data connection | Transfer complete |
| 227 | Entering passive mode | Passive mode negotiation |
| 230 | User logged in | Authentication successful |
| 250 | Requested file action okay | Success |
| 257 | Pathname created | Success |
| 331 | User name okay, need password | Valid username exists |
| 332 | Need account for login | Additional info required |
| 350 | Requested file action pending | Informational |
| 421 | Service not available | Service down |
| 425 | Can't open data connection | Firewall likely blocking |
| 426 | Connection closed | Transfer interrupted |
| 450 | File unavailable | File exists but no access |
| 451 | Requested action aborted | Local error |
| 452 | Insufficient storage space | Disk full on server |
| 500 | Syntax error | Invalid command |
| 501 | Syntax error in parameters | Invalid argument |
| 502 | Command not implemented | Unsupported command |
| 503 | Bad sequence of commands | Wrong command order |
| 504 | Command not implemented for that parameter | Parameter not supported |
| 530 | Not logged in | Authentication failed / Anonymous disabled |
| 532 | Need account for storing files | Account required |
| 550 | File unavailable | File does not exist or no access |
| 552 | Exceeded storage allocation | Disk quota exceeded |
| 553 | File name not allowed | Invalid filename |

---

## 6. Common FTP Vulnerabilities

| Vulnerability | Description | Impact |
|---------------|-------------|--------|
| Plain Text Authentication | Username and password sent in clear | Credential sniffing (Wireshark) |
| Anonymous Authentication Enabled | Anyone can login with "anonymous:anonymous" | Unauthorized access |
| Default Credentials | admin:admin, root:root, ftp:ftp | Easy compromise |
| Directory Traversal | Access files outside FTP root using ../../../ | Data breach |
| Weak Password Policy | Easy passwords | Brute force success |
| Missing Encryption | Data transferred in plain text | Data sniffing |
| Insecure Permissions | World-writable directories | Malware upload |
| FTP Bounce Attack | Using FTP server to scan other hosts | Network scanning |
| Brute Force Attack | Repeated login attempts | Account compromise |

---

## 7. FTP Security Testing

### Reconnaissance & Enumeration

```bash
# Scan for FTP service
nmap -p 21 <target>

# FTP version detection
nmap -sV -p 21 <target>

# Check for anonymous login
nmap -p 21 --script ftp-anon <target>

# FTP brute force with Hydra
hydra -l admin -P /usr/share/wordlists/rockyou.txt ftp://<target>

# FTP bounce attack scan
nmap -b ftpuser:pass@<ftp-server> <target>
