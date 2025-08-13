# 🔐 OpenVPN Setup on DigitalOcean

A comprehensive guide to set up your own VPN server uson DigitalOcean to bypass DNS restrictions and secure your internet connection.

## 📋 Prerequisites

- DigitalOcean account
- Basic knowledge of Linux command line
- SSH client on your local machine

## 🚀 Setup Instructions

### 1. Create the VPS

1. Go to [DigitalOcean](https://www.digitalocean.com/) and create a **Ubuntu 22.04** droplet
2. Choose your preferred size (Basic $6/month is sufficient for personal use)
3. Add your SSH key or use root password authentication
4. Create and wait for the droplet to be ready

### 2. Create a Non-Root User

```bash
# Create a new user (replace 'eze' with your preferred username)
adduser eze

# Add the user to the sudo group
usermod -aG sudo eze
```

### 3. SSH into the Server

Connect to your server as the newly created user:

```bash
ssh eze@your_server_ip
```

### 4. Install and Configure OpenVPN

Run the following commands on your VPS:

```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Download the OpenVPN install script
wget https://git.io/vpn -O openvpn-install.sh
chmod +x openvpn-install.sh

# Run the script
sudo ./openvpn-install.sh
```

**Important:** 
- Follow the prompts (default settings are usually fine)
- The script will create a `.ovpn` client configuration file (e.g., `client.ovpn`)
- Remember the location of this file for the next step

### 5. Download the Client Configuration

From your **local computer**, download the OpenVPN configuration file:

```bash
scp eze@your_server_ip:/root/client.ovpn .
```

## 📱 Connecting to Your VPN

### Linux

```bash
sudo openvpn --config client.ovpn
```

### Windows/Mac

1. Download and install [OpenVPN Connect](https://openvpn.net/client-connect-vpn-for-windows/)
2. Import the `client.ovpn` file
3. Click "Connect"

### Mobile (Android/iOS)

1. Install the **"OpenVPN Connect"** app from your app store
2. Import the `client.ovpn` file
3. Tap "Connect"

## ✅ Verify Your Connection

Test that your VPN is working properly:

Import client.ovpn and connect.

Android/iOS:

Install the “OpenVPN Connect” app.

Import client.ovpn and connect.

7. Verify connection
bash
Copy
Edit
```bash
curl ifconfig.me
```

**Expected result:** This should show your VPS IP address, not your local ISP IP.

## 🔌 Disconnecting

- **Linux:** Press `Ctrl + C` in the OpenVPN terminal
- **Other devices:** Click "Disconnect" in the OpenVPN app

## ⚙️ Optional: Auto-Start VPN on Boot

You can configure OpenVPN to start automatically using systemd. This ensures your VPN connection is established on system startup.

## 🔄 VPN vs Reverse Proxy

Understanding the difference between your VPN setup and a reverse proxy:

### Your VPN Setup
- **Purpose:** All traffic from your device is routed through the VPS
- **Benefit:** Hides your real IP and bypasses DNS/network restrictions

### Reverse Proxy
- **Purpose:** Sits in front of a server to forward client requests
- **Use cases:** Load balancing, security, caching

## 📊 Network Diagrams

### VPN Architecture (Your Setup)

```
[Your Device] 
     |
     | (Encrypted Tunnel)
     v
[Your VPS with OpenVPN] ──→ [Internet / Websites / DNS]
```

### Reverse Proxy Architecture

```
[Client / User]
     |
     v
[Reverse Proxy Server] ──→ [Backend Server / Website]
```

## 🎯 Key Benefits

- ✅ **Privacy:** Hide your real IP address
- ✅ **Security:** Encrypt all your internet traffic
- ✅ **Bypass restrictions:** Access geo-blocked content
- ✅ **DNS bypass:** Circumvent DNS-based filtering
- ✅ **Cost-effective:** ~$6/month for your own private VPN

---

**Note:** Remember to keep your VPS updated and monitor its usage to avoid unexpected charges.

8. Disconnect
On Linux: Press CTRL + C in the OpenVPN terminal.

On other devices: Click “Disconnect” in the app.

9. Optional: Auto-start VPN on boot
Can be set up using systemd to run OpenVPN automatically.

10. VPN vs Reverse Proxy
Your setup is a VPN: all traffic from your device is routed through the VPS.

Reverse proxy: sits in front of a server to forward client requests, used for load balancing or security.

Diagram:

VPN (Your setup)

pgsql
Copy
Edit
[Your Device] 
     |
     |  (Encrypted Tunnel)
     v
[Your VPS with OpenVPN]  --->  [Internet / Websites / DNS]
Reverse Proxy

pgsql
Copy
Edit
[Client / User]
     |
     v
[Reverse Proxy Server]  --->  [Backend Server / Website]
VPN hides your real IP and bypasses DNS/network restrictions.

Reverse proxy hides backend servers and manages traffic for them.