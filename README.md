# IIS Server Role Configuration — Windows Server 2025 (Desktop & Core)

## 1. Overview

This project documents the full process of configuring Microsoft IIS on both **Windows Server 2025 Desktop and Core** editions.

Performed hands-on by **Jaden Maciel-Shapiro**, it demonstrates hosting multiple websites locally, editing the `hosts` file for name resolution, and enabling remote IIS management for a headless Core server.

---

## 2. Problem / Objectives

* Deploy and validate the IIS (Web Server) role on Windows Server 2025.

* Host multiple websites with custom domain bindings.

* Configure host name resolution without DNS by editing `hosts`.

* Enable remote IIS Management for a Server Core installation.

* Validate connectivity and site availability from the Desktop VM.

---

## 3. Environment & Prerequisites

* **Host platform:** Oracle VirtualBox 7.x

* **Guest OS:** Windows Server 2025 Desktop and Server 2025 Core

* **Networking:** Internal Network mode

* **Browsers:** Internet Explorer / Edge (compatibility mode)

* **Tools:** PowerShell 7.x, Registry Editor, Notepad

* **Files created:**

  * `C:\inetpub\wwwroot\index.html`

  * `C:\inetpub\wwwroot\mycoolwebsite\index.html`

  * `C:\inetpub\wwwroot\mynewsite\index.html`

---

## 4. Steps Summary

### Windows Server 2025 Desktop

1. Installed the **Web Server (IIS)** role using Server Manager → *Add Roles and Features*.

2. Verified successful install by browsing to `http://localhost`.

3. Created a custom `index.html` with `<h3>Jaden Maciel-Shapiro</h3>` in `C:\inetpub\wwwroot`.

4. Deleted default `iisstart.html` and `iisstart.png`.

5. Opened **IIS Manager** → verified `index.html` listed under *Default Document*.

6. Confirmed page served correctly at `http://localhost`.

7. Created second website via *Sites > Add Website*:

   * Physical Path: `C:\inetpub\wwwroot\mycoolwebsite`

   * Host Name: `www.mycoolwebsite.com`

8. Edited `C:\Windows\System32\drivers\etc\hosts` to map:

   ```text

   192.168.12.1    www.mycoolwebsite.com

   192.168.12.1    www.cybr2650

   ```

9. Verified `http://www.mycoolwebsite.com` loads distinct content.

### Windows Server 2025 Core

10. Installed **IIS role** on the Core VM remotely via Server Manager.

11. In PowerShell (Core):

    ```powershell

    Install-WindowsFeature Web-Mgmt-Service

    netsh advfirewall firewall add rule name="IIS Remote Management" dir=in action=allow service=WMSVC

    Set-Service -Name WMSVC -StartupType Automatic

    Start-Service WMSVC

    ```

12. Edited Registry:

    * Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WebManagement\Server`

    * Set `EnableRemoteManagement=1`

13. Created new folder:

    ```powershell

    mkdir C:\inetpub\wwwroot\mynewsite

    ```

14. From Desktop Server, connected IIS Manager → *Connect to Server* → entered Core machine name.

15. Verified connection and site tree expand successfully.

16. Added new website on Core:

    * Site Name: `mynewsite`

    * Physical Path: `C:\inetpub\wwwroot\mynewsite`

    * Host Name: `www.mynewsite.com`

17. On Core VM, created new web page:

    ```html

    <h3>Jaden Maciel-Shapiro</h3>

    <p>Server Core Webserver</p>

    ```

18. Edited hosts file on Desktop VM to map Core IP → `www.mynewsite.com`.

19. Verified custom Core page loads in IE via domain name.

---

## 5. Evidence (Text-Only)

### **Evidence 01 — Localhost Placeholder**

* *What would be visible:* Internet Explorer showing `http://localhost` and IIS default placeholder page.

* *Text excerpt:*

  ```text

  IIS Windows Server

  ```

* *Why this matters:* Confirms IIS role installed and serving default content.

---

### **Evidence 02 — Custom Index File in Root**

* *What would be visible:* File Explorer listing only one file in `C:\inetpub\wwwroot`.

* *Text excerpt:*

  ```text

  index.html    1 KB    HTML Document

  ```

* *Why this matters:* Confirms removal of defaults and correct naming.

---

### **Evidence 03 — Custom Site Display**

* *What would be visible:* IE showing URL `http://localhost` with page text.

* *Text excerpt:*

  ```html

  <h3>Jaden Maciel-Shapiro</h3>

  ```

* *Why this matters:* Confirms the personalized web page is served correctly.

---

### **Evidence 04 — Alternate Website Binding**

* *What would be visible:* IE URL bar `http://www.mycoolwebsite.com` displaying alternate content.

* *Text excerpt:*

  ```text

  Welcome to My Cool Website

  ```

* *Why this matters:* Shows virtual hosting works using host headers.

---

### **Evidence 05 — Core Server Placeholder**

* *What would be visible:* Browser showing Core server IP (`http://192.168.12.x`) with IIS default page.

* *Text excerpt:*

  ```text

  IIS Windows Server

  ```

* *Why this matters:* Verifies IIS on Core responds to HTTP requests.

---

### **Evidence 06 — Core Custom Site**

* *What would be visible:* Browser URL `http://www.mynewsite.com` displaying new page.

* *Text excerpt:*

  ```html

  <h3>Jaden Maciel-Shapiro</h3>

  <p>Server Core Webserver</p>

  ```

* *Why this matters:* Confirms functional site hosted on Core and remotely managed.

---

## 6. Results & Validation

* Default and custom sites load successfully via hostnames.

* Host header configuration and manual `hosts` mapping verified.

* Remote IIS management from Desktop to Core succeeded.

* HTML pages correctly served from respective directories.

---

## 7. Timeline / Activity Dates

* **2025-10-29 —** Installed IIS role on Desktop Server 2025.

* **2025-10-30 —** Created and verified custom site `www.mycoolwebsite.com`.

* **2025-11-01 —** Installed and configured IIS on Server Core 2025.

* **2025-11-02 —** Connected IIS Manager remotely and verified Core site.

---

## 8. What I Learned

* Installed and managed IIS roles on both Desktop and Core editions.

* Used PowerShell and Registry edits to enable remote IIS management.

* Practiced host header configuration and multi-site hosting.

* Validated services using text-only terminal and browser inspection.

* Reinforced understanding of internal networking and local name resolution.

---

## 9. Troubleshooting Notes

* Needed to **run PowerShell as Administrator** to install IIS features.

* Initial **firewall block** prevented remote IIS Manager connection → fixed via `netsh` rule.

* **Hosts file permissions** required elevated Notepad.

* **Registry key** path typo (`Server` vs `Servers`) corrected manually.

* **Service not starting** resolved by setting `WMSVC` startup type to Automatic.

---

## 10. Author / Ownership

This project was performed hands-on by **Jaden Maciel-Shapiro** during late October–early November 2025.

It demonstrates full deployment and remote management of IIS Web Server on both Windows Server 2025 Desktop and Core.

Repository created for professional portfolio and technical reference.

---
