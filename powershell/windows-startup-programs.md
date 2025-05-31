# Understanding How Programs Use Windows Startup Features #

**This is an introductory article on how to manage programs that start automatically when you login.**

Answers questions like

> 1. How do I stop a program from starting automatically at login?
> 2. Are there different ways program can be started automatically?
> 3. What if I can't find a program that seem to start automatically but I can't see it in the startup up folder or in the Task Manager menu?
> 4. Are there advanced utilities to dig deeper?
> 5. What are some references where I can read or learn more?

Windows provides several mechanisms that allow programs to automatically launch when the system starts or when a user logs in. These features are widely used for convenience, background services, update agents, and essential system utilities. 

This article explores the most common and uncommon methods by which programs use Windows startup features, including an expanded explanation of the Windows Registry “Run” keys and a real-world example using Discord.

This also introduces a program called autoruns.

**Autoruns** is a free utility developed by **Microsoft's Sysinternals team**, designed to display everything that starts up automatically with Windows. This includes programs in the **startup folder, registry Run keys, Task Scheduler, services, drivers, browser helper objects,** and more.

---

## Common Windows Startup Mechanisms

### 1. Startup Folder (User or Common)

Programs can place shortcuts into special startup folders that Windows checks at login:

* **User-specific**: `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup`
* **All users**: `%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\Startup`

Any shortcut placed in these folders will be launched automatically when the respective user logs in.

### 2. Windows Registry "Run" Keys

The **Registry "Run" keys** are among the most widely used startup methods. These keys are:

* `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
* `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`

The word **"Run"** in this context refers to "run this program at logon." When a user logs in, Windows checks these keys and executes any values listed under them. Each value typically consists of a name and the path to an executable:

```reg
"MyApp"="C:\\Program Files\\MyApp\\myapp.exe"
```

* Entries under `HKEY_CURRENT_USER` are specific to the currently logged-in user.
* Entries under `HKEY_LOCAL_MACHINE` apply to all users and usually require administrative privileges to modify.

These keys are a popular target not only for software vendors but also for malware authors due to their simplicity and reliability.

Programs listed here will **usually appear in Task Manager’s Startup tab**, especially if the application includes proper metadata or is digitally signed. However, some manually added or lightweight entries may not show up in the Startup tab but will still run.

#### How to List Programs Under the "Run" Keys

You can view the entries under these keys using:

* The **Registry Editor**: Run `regedit` and navigate to the respective keys.
* A **PowerShell command**:

```powershell
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"
```

* **Autoruns** utility from Sysinternals, which provides a full graphical overview of all startup locations.

### 3. Task Scheduler

Applications may use the **Task Scheduler** to create tasks that run at login, at startup, or on specific triggers like network availability. Scheduled tasks provide more control than registry entries, such as specifying conditions, delays, or required privileges.

### 4. Windows Services

Programs that must start in the background or before any user logs in—like antivirus software—are typically registered as **Windows Services**. Services are managed through the Service Control Manager and can start automatically, manually, or based on triggers.

### 5. App Settings (In-App Configuration)

Many modern applications include a "launch at startup" setting within their own UI. When enabled, the application may internally choose to add a Run key, create a scheduled task, or place a shortcut in the startup folder. Users may not always be aware of which method is being used.

---

## Uncommon or Advanced Startup Mechanisms

### 1. Group Policy Startup Scripts

System administrators in enterprise environments can configure Group Policy Objects (GPOs) to run scripts at startup or logon. These scripts are often used for maintenance tasks or enforcing configuration standards.

### 2. COM Objects and Shell Extensions

Some programs register COM components or shell extensions that are invoked during shell startup, providing highly integrated behavior at the cost of greater complexity.

### 3. Services with Trigger Start

Introduced in later versions of Windows, trigger-start services allow programs to start based on system events (e.g., user presence, network changes) instead of during the boot or logon sequence.

### 4. WMI Event Subscriptions

A rarely used and more advanced mechanism, programs can create WMI event subscriptions that cause a script or binary to run when specific system conditions are met. This method is sometimes used by persistent malware due to its stealthy nature.

---

## Case Study: Discord

**Discord**, a popular chat and gaming application, uses a hybrid startup approach:

* By default, Discord is set to launch at startup through an internal setting available under **User Settings > Windows Settings**.
* When enabled, this setting results in a Run key entry being added to `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`.
* Discord has also been known to use the Windows **Task Scheduler** for startup in certain installations.
* Because Discord manages this internally, disabling it via the Task Manager’s Startup tab may be ineffective if the in-app setting remains enabled.

**To disable Discord from launching at startup:**

1. Open Discord.
2. Navigate to **User Settings > Windows Settings**.
3. Turn off the **"Open Discord"** toggle.

If the behavior persists, users can inspect the Registry or Task Scheduler directly.

---

## Autoruns: A Deep Dive Into Startup Entries

**Autoruns** is a free utility developed by **Microsoft's Sysinternals team**, designed to display everything that starts up automatically with Windows. This includes programs in the startup folder, registry Run keys, Task Scheduler, services, drivers, browser helper objects, and more.

### Why Use Autoruns?

Autoruns provides a comprehensive view of all auto-starting locations that even Task Manager doesn’t show. It’s particularly valuable for:

* Diagnosing slow startups
* Identifying malware persistence
* Cleaning up old or unused background entries

### How to Install Autoruns

1. Visit the official Microsoft page: [Autoruns for Windows](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)
2. Download the ZIP file.
3. Extract it, then run `Autoruns.exe` (for 32-bit) or `Autoruns64.exe` (for 64-bit).
4. Run as Administrator for full system visibility.

### How to Use Autoruns

* The **Logon** tab shows entries that run at user login.
* You can **search** (Ctrl + F) for a specific app, e.g., “Discord.”
* **Uncheck** a box to disable a startup item.
* **Right-click** to jump to the Registry location or delete the entry entirely.

Be cautious when disabling unknown entries — Autoruns does not prompt for confirmation.

---

## Best Practices for Managing Startup Programs

* Use **Task Manager** or **Settings > Apps > Startup** to manage common startup entries.
* Use **Autoruns** from Sysinternals to identify and disable deeply embedded or hidden startup configurations.
* Before removing registry entries or scheduled tasks, verify their purpose to avoid breaking needed functionality.
* Keep an eye on unfamiliar startup entries, which can be a sign of unwanted software or malware.

---

## References and Further Reading

1. [Microsoft Docs - Manage Startup Apps](https://www.microsoft.com/en-us/windows/learning-center/take-control-of-windows-startup) — Overview of how to manage apps that run automatically in Windows 11, including how to turn them on or off.
2. [Sysinternals Autoruns Tool](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns) — Download and documentation page for Autoruns, a powerful utility to see everything configured to run at startup.
3. [Windows Registry Run Keys Explained - HowToGeek](https://www.howtogeek.com/74523/how-to-disable-startup-programs-in-windows/) — Detailed guide explaining various startup methods, especially registry-based ones.
4. [Discord Support - Settings Overview](https://support.discord.com/) — Discord's support portal for managing application behavior, including how to control its startup settings.

---

By understanding how startup mechanisms work and how various programs like Discord implement them, users and IT professionals can maintain control over system behavior, performance, and security.
