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
./MagicDump -t <target_ip> -u <username> -p <password>
```

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
./MagicDump -t 192.168.1.100 -u admin -p password123
```

<br>

Output:
```
[INFO] Connecting to target 192.168.1.100 via SMB...
[LOG] Operation: SAM, Status: success
[INFO] SAM dump saved to /home/user/.magicdump/dumps/SAM_dumped_192.168.1.100.txt
[LOG] Operation: LSASS, Status: success
[INFO] LSASS dumped and decrypted successfully
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
