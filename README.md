# 📁 FTP (File Transfer Protocol) – Complete Guide for Security Testing

FTP is one of the oldest and most widely used protocols for transferring files between a client and a server. This guide covers everything from basic concepts to security testing and hardening.

---

## 📌 Table of Contents

1. What is FTP?
2. How FTP Works (Active vs Passive Mode)
3. FTP vs SFTP vs FTPS
4. FTP Commands
5. FTP Response Codes
6. Common FTP Vulnerabilities
7. FTP Security Testing
8. FTP Hardening (Defense)
9. Commands Cheatsheet
10. Hands-on Labs

---

## 1. What is FTP?

**FTP (File Transfer Protocol)** is a standard network protocol used to transfer files between a client and a server on a computer network.

- **Ports:**
  - Port 21 – Control Connection (commands, authentication)
  - Port 20 – Data Connection (actual file transfer)
- **Authentication:** Username and password (plain text)
- **Stateless:** Each command is independent

### How FTP Works
