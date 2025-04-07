# 🛠️ Fixing VS Code Extension Install Failures: Deep Dive & Winsock Reset Recovery

> **Disclaimer:** This document was prepared with assistance from ChatGPT. While care was taken to ensure accuracy, please validate technical details independently where necessary.

## ❗ Problem Summary
Visual Studio Code (VS Code) failed to install extensions, throwing errors like:
- `Error while fetching extensions. Failed to fetch`
- `net::ERR_CONNECTION_RESET`
- `No default agent registered`
- Frequent `Extension Host is unresponsive` events

Despite:
- Browsers working normally (extensions marketplace URL reachable)
- No VPN, proxy, or third-party antivirus
- A clean reinstall of VS Code

---

## ✅ Steps Taken (And Eliminated)

### 🔍 System & Network-Level Checks
- ✅ Verified network marked as **Private**, not Public
- ✅ Switched to **mobile hotspot** to rule out router/ISP
- ✅ Restarted router — no effect
- ✅ Verified no VPN, proxy, or special DNS configurations

### 🔍 VS Code Checks
- ✅ Uninstalled and reinstalled VS Code
- ✅ Fully cleared config and cache directories:
  - `%APPDATA%\Code`
  - `%USERPROFILE%\.vscode`
  - `%LOCALAPPDATA%\Programs\Microsoft VS Code`
- ✅ Set VS Code settings:
```json
"http.proxy": null,
"http.proxyStrictSSL": false
```
- ✅ Disabled firewall for test

### 🔍 Environment/Proxy Validation
- ✅ Verified:
```bash
netsh winhttp show proxy  # Output: Direct access
```
- ✅ Confirmed no `NODE_OPTIONS`, `HTTP_PROXY`, `HTTPS_PROXY` env vars

### 🔍 Certificate and TLS Checks
- ✅ Certificates present in `certmgr.msc`
- ✅ Browsers could reach: `https://marketplace.visualstudio.com/_apis/public/gallery/extensionquery`
- ✅ Even `--ignore-certificate-errors` didn’t help VS Code

---

## 💣 Final Clue: "No default agent registered"
This VS Code error means its internal Node.js/Electron-based HTTPS system failed to initialize a usable agent — usually due to:
- Broken TLS stack
- Proxy misrouting
- Winsock corruption

---

## ✅ **The Fix**: Winsock Reset

### 🔧 Command:
```bash
netsh winsock reset
```
Followed by a full **system reboot**.

### ✅ Result:
- VS Code Extensions panel worked again
- PowerShell extension and others installed from Marketplace
- No more connection reset or TLS agent errors

---

## 🧠 What is Winsock?
**Winsock (Windows Sockets)** is the Windows API that allows applications to communicate over TCP/IP networks. It handles:
- DNS resolution
- Socket creation and teardown
- Protocol management
- TCP/IP communication for many apps (including Node.js, Electron, browsers, and more)

### 🔧 What `netsh winsock reset` Does:
- Restores the Winsock catalog to its default configuration
- Clears third-party LSPs (Layered Service Providers)
- Removes custom TCP/IP protocol hooks or corrupted entries

### ❗ How It Gets Corrupted:
- Partial network driver updates
- VPN or proxy tools installing/removing LSPs improperly
- Malware modifying low-level networking behavior
- Registry corruption
- Power outages/crashes during network-intensive processes
- Improper uninstallers (network monitoring apps, proxies, or filters)

### 🔁 Resulting Symptoms Can Include:
- Only *some* apps (like VS Code, Discord, GitHub Desktop) can't connect, while browsers work fine
- Repeated `net::ERR_CONNECTION_RESET` errors
- Flaky behavior: sometimes connections work, other times fail
- Marketplace and extensions failing to load or sync
- Electron-based apps failing silently or freezing unexpectedly

---

## 🔐 Understanding Network and Firewall Configuration in Windows

### 🔍 Checking Network Type (Private vs Public)
1. Go to **Settings > Network & Internet > Wi-Fi or Ethernet**
2. Click the name of the network you are connected to
3. Under **Network Profile**, check if it is set to **Public** or **Private**

**Difference between types:**
- **Private**: Trusted networks (home, work). File/printer sharing and app access are more permissive.
- **Public**: Untrusted networks (cafes, hotels). Most inbound connections are blocked by default.

**How to know what you're on:**
- Windows shows the current profile under the connection’s name in **Network & Internet** settings
- Your **home Wi-Fi** should typically be set to **Private** for smooth app connectivity

### 🔥 Accessing Windows Firewall Settings
1. Press `Windows + S`, type **"Windows Security"**, and open it
2. Go to **Firewall & network protection**
3. You'll see options for **Domain**, **Private**, and **Public** network profiles
4. Click the active network → Toggle firewall on/off

### 🔐 Allowing VS Code Through the Firewall
1. In **Firewall & network protection**, click **"Allow an app through firewall"**
2. Click **Change settings** (admin required)
3. Find **"Code"** or **"Visual Studio Code"** in the list
   - If not listed, click **"Allow another app"** and **browse for the VS Code executable**

### 🔎 Finding VS Code Executable with PowerShell
```powershell
Get-Command code | Select-Object Source
```
Or if installed in default path:
```
C:\Users\<username>\AppData\Local\Programs\Microsoft VS Code\Code.exe
```

---

## 🧾 Checking Certificate Validity in Windows
1. Press `Win + R`, type `certmgr.msc`, press Enter
2. Navigate to **Trusted Root Certification Authorities > Certificates**
3. Look under the **"Status"** column
   - If blank, the certificate is usually valid
   - If any cert says **"Not trusted"** or **"Revoked"**, that's a red flag
4. Double-click any certificate to view validity period and intended purposes

---

## 🛠️ Using `netsh` to Debug Network Issues

### ✅ Check proxy settings:
```bash
netsh winhttp show proxy
```
Use this to confirm if a system proxy is interfering with your app. If something is set, it may route HTTPS through a proxy that blocks or resets connections.

### ✅ Reset Winsock stack:
```bash
netsh winsock reset
```
Fixes issues where socket handlers or TCP/IP extensions have become corrupt, especially after uninstalling VPNs, proxies, or network tools.

### ✅ View TCP global settings:
```bash
netsh interface tcp show global
```
This command shows system-wide TCP behavior settings, such as congestion control, auto-tuning level, and other optimizations. Useful to identify if tuning options are limiting throughput or responsiveness.

### ✅ Reset all TCP/IP settings:
```bash
netsh int ip reset
```
Resets the full TCP/IP stack and clears any custom configuration applied by third-party tools or faulty drivers.

### ✅ Trace and diagnose connection issues:
```bash
netsh trace start scenario=InternetClient capture=yes
netsh trace stop
```
Generates a deep diagnostic ETL trace file showing every packet and system decision related to your internet client usage. Use tools like Microsoft Network Monitor to interpret results and uncover dropped connections, DNS issues, or handshake failures.

---

## ✅ Final Takeaway
If your browser can connect to a site but VS Code (or another Electron-based app) fails with errors like `net::ERR_CONNECTION_RESET` or `Failed to fetch` — and you've ruled out proxy, certs, firewall, and reinstalling:

> 🔧 Run: `netsh winsock reset` → Reboot → Test again

It can resolve silent, deep issues that **only affect certain kinds of apps**, not your entire system.

---

## 📚 Helpful References
- [Winsock reset explained (Microsoft)](https://support.microsoft.com/help/299357) — Covers what Winsock is, how to reset it, and when it helps.
- [Visual Studio Code network troubleshooting](https://code.visualstudio.com/docs/setup/network) — Official tips from Microsoft on how VS Code handles proxies and networking.
- [Node.js TLS documentation](https://nodejs.org/api/tls.html) — Details on how Node handles secure connections and certificate validation.
- [Electron networking and proxy-agent](https://github.com/microsoft/vscode-proxy-agent) — GitHub repo for the VS Code-specific proxy agent logic and its known issues or updates.
- [Windows Defender Firewall: Allow apps](https://support.microsoft.com/windows/allow-an-app-through-windows-defender-firewall) — Guide to adding exceptions for specific programs like VS Code.
- [Manage trusted root certificates in Windows](https://learn.microsoft.com/en-us/windows/security/identity-protection/management/manage-trusted-root-certificates) — Describes certificate stores and trust chain validation.
- [Use netsh to view and troubleshoot TCP/IP](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/use-netsh-commands-to-view-or-modify-tcp-ip-settings) — Microsoft's guide to `netsh` usage for network troubleshooting.
- [Change a Wi-Fi network from public to private](https://support.microsoft.com/windows/make-a-wi-fi-network-public-or-private-in-windows) — How to set the appropriate network profile in Windows for better app and firewall compatibility.

