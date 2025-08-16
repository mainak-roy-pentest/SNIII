# 🎣(SNIII) 
# Phishing Simulator for Penetration Testing

*A tool for authorized security testing and awareness training.*

---

## 📌 Overview
This tool simulates phishing attacks for **authorized penetration testing** and **security awareness training**. It creates fake login pages for popular services (Instagram, Facebook, GitHub) and logs submitted credentials locally. 

**Disclaimer:** Unauthorized use of this tool is illegal and unethical. Use only with explicit permission.

---

## 🛠 Features
- **Multi-service phishing templates**: Instagram, Facebook, GitHub.
- **Real-time credential logging**: Captures credentials in `logs/creds.txt`.
- **Public URL generation**: Uses `ngrok` to expose pages for remote testing.
- **Responsive design**: Mimics real login pages.

---

## ⚙️ Prerequisites
- Linux/macOS (Windows support untested)
- PHP (`php -v` to check)
- `ngrok` ([Download here](https://ngrok.com/download))
- `jq` (for parsing ngrok URLs)
  ```bash
  sudo apt install jq  # Debian/Ubuntu
  brew install jq      # macOS

---

## 🚀 Installation & Usage
1. Clone the Repository
```bash
git clone https://github.com/yourusername/phishing-simulator.git
cd phishing-simulator
```

2. Run the Tool
 ```bash
chmod +x phish_simulator.sh
./phish_simulator.sh
```
3. Select a Target
Choose from the menu:
```bash
1. Instagram
2. Facebook
3. GitHub
4. Exit
```
4. Access the Phishing Page
- Local URL: http://127.0.0.1:[PORT]
- Public URL: Automatically generated via ngrok (e.g., https://abc123.ngrok.io).

## 📂 File Structure
```bash
.
├── sites/
│   ├── instagram/
│   │   ├── index.html       # Fake login page
│   │   ├── submit.php       # Credential handler
│   │   └── logs/creds.txt   # Stored credentials
│   ├── facebook/            # Same structure as above
│   └── github/              # Same structure as above
└── phish_simulator.sh       # Main script
```
## 🔒 Ethical & Legal Notice
- Authorized Use Only: This tool is for legitimate security testing with explicit permission.
- No Malicious Activity: Do not use against systems you do not own or have permission to test.
- Compliance: Adhere to laws like GDPR, CFAA, and local regulations.


## ❓ FAQ
Q: How do I stop the server?
Press CTRL+C in the terminal.

Q: Why is ngrok not working?
Ensure ngrok is installed and authenticated:
```bash
ngrok authtoken YOUR_AUTH_TOKEN
```
Q: Can I add more services?
Yes! Clone a template from sites/instagram/ and modify index.html/submit.php.
## 📬 Contact
For questions or ethical concerns:

📧 Email: mainakr455@gmail.com
