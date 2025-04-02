<!-- ********************* -->
# Installation Gotchas with VS Code
<!-- ********************* -->

Visual Studio Code is widely used for its speed and extensibility, but its installation options can introduce unexpected behavior. This guide walks through the common pitfalls ("gotchas") related to installing VS Code, including differences between installers, conflicts with admin permissions, and how configuration files are handled.

<!-- ********************* -->
# 1. What Are the Two Different Installers?
<!-- ********************* -->

VS Code provides two primary installers for Windows:

- **User Installer**: Designed for a single user. Installs in the user's profile directory.
- **System Installer**: Designed for all users on the machine. Installs in `Program Files` and requires administrative privileges.

Each is intended for different use cases and behaves differently when it comes to permissions and updates.

<!-- ********************* -->
# 2. Are They Two Different Executables?
<!-- ********************* -->

Yes, they are two separate executables:

- **User Installer**: Named `VSCodeUserSetup-x64-<version>.exe`
- **System Installer**: Named `VSCodeSetup-x64-<version>.exe`

They are not interchangeable and install to different locations with distinct scopes.

<!-- ********************* -->
# 3. Where Can You Find Each Installer?
<!-- ********************* -->

Both installers are available from the official VS Code website:

- Main download page: [https://code.visualstudio.com/Download](https://code.visualstudio.com/Download)
- Alternate downloads page: [https://code.visualstudio.com/#alt-downloads](https://code.visualstudio.com/#alt-downloads)

The alternate page clearly labels the User and System installers.

<!-- ********************* -->
# 4. Do I Need to Uninstall the Previous Version?
<!-- ********************* -->

Yes, if you're switching between the User and System installer, it is recommended to uninstall the previous version first. Otherwise, you might end up with two installations, which can cause confusion and potential conflicts.

Uninstalling VS Code will not delete your settings, extensions, or snippet files unless explicitly chosen.

<!-- ********************* -->
# 5. What Happens to Global Config Files Like Snippets?
<!-- ********************* -->

VS Code stores user-level configuration files such as settings, extensions, and snippets in:

```plaintext
C:\Users\<YourUsername>\AppData\Roaming\Code\User
```

This includes:

- `settings.json`
- `keybindings.json`
- Custom snippets (`*.code-snippets`)

These files persist through uninstall/reinstall operations unless you manually delete them.

<!-- ********************* -->
# 6. What Are the Disadvantages of the User Installer?
<!-- ********************* -->

Even on a personal laptop, the User Installer has a few limitations:

- **Cannot auto-update if run as Administrator**
- **Not available for other users on the machine**
- **May cause issues in environments with elevated permissions or script-based setups**

It’s more convenient for locked-down environments, but less flexible for power users.

<!-- ********************* -->
# 7. What Is the Conflict When Running as Admin?
<!-- ********************* -->

If you run the **User Installer version** of VS Code with **"Run as Administrator"**, updates are disabled. You’ll see this warning:

> "Updates are disabled because you are running the user-scope installation of Visual Studio Code as Administrator."

This happens because the update mechanism tries to write to user-specific locations, but it's running in an elevated context where it no longer has access to those.

The **System Installer**, on the other hand, supports being run as Administrator and updates without issue.

<!-- ********************* -->
# 8. Firewall Considerations
<!-- ********************* -->

When running behind strict corporate firewalls or using extensions that require internet access (e.g., Live Share, GitHub Copilot), the **System Installer** can have advantages:

- **Admin-level network exceptions**: It's easier to create persistent inbound/outbound firewall rules for applications in `Program Files`, which are recognized as trusted locations.
- **Fewer issues with Windows Defender SmartScreen or third-party antivirus**, which may restrict per-user apps running in AppData.
- **Better compatibility with IT-managed group policies**, where system-level applications are whitelisted but user-level ones are not.

If you anticipate using networked features, remote development, or extensions that reach external services, the System Installer is generally more firewall-friendly.

<!-- ********************* -->
# 9. How to Tell Which Version You're Running and Where It's Installed
<!-- ********************* -->

To check whether you're running the User or System version:

- **Check the install path**:
  - User Installer: `C:\Users\<YourUsername>\AppData\Local\Programs\Microsoft VS Code`
  - System Installer: `C:\Program Files\Microsoft VS Code`

- **In PowerShell or Command Prompt**, run:
  ```powershell
  Get-Command code | Select-Object Source
  ```
  This will show the path to `code.cmd`, which is inside the installed directory.

- **In Windows Settings > Apps**, look for "Visual Studio Code (User Installer)" or simply "Visual Studio Code".

These checks help you determine how VS Code was installed and which update behavior you can expect.

<!-- ********************* -->
# 10. Is It Okay to Install VS Code in a Custom Directory?
<!-- ********************* -->

Yes, you can choose a custom installation path during setup, even with the System Installer. This may be useful for portable use cases or organizing non-default paths. However, there are trade-offs:

### Pros:
- Allows better control over folder structure
- Useful for portable or standalone installs
- Helpful in environments where `Program Files` is restricted

### Cons:
- May not be recognized by system tools or scripts expecting the default path
- Could lead to confusion if both default and custom installations exist
- Firewall and security rules may treat custom locations as less trusted

Unless you have a specific reason, the default installation locations are recommended for consistency and compatibility.

<!-- ********************* -->
# 11. Using `Get-Command` to Locate VS Code and Other Tools
<!-- ********************* -->

The PowerShell command `Get-Command` is a useful way to determine how a command is being resolved in your environment. For example, running:

```powershell
Get-Command code
```

will return details about the `code` command — including whether it’s an application, function, alias, or script — and where it lives.

### Example Output:
```plaintext
CommandType     Name   Version    Source
-----------     ----   -------    ------
Application     code   0.0.0.0    C:\Users\YourName\AppData\Local\Programs\Microsoft VS Code\bin\code.cmd
```

This reveals that VS Code is installed in your user profile directory and is accessed via `code.cmd`. It's particularly useful for checking which version of a tool is active when multiple are installed.

You can also extract just the path using:
```powershell
(Get-Command code).Source
```

This works for any other tool available on your `PATH`, such as:

```powershell
Get-Command git
Get-Command node
Get-Command python
```

It helps troubleshoot unexpected behavior caused by conflicting versions or unregistered binaries.

<!-- ********************* -->
# 12. References
<!-- ********************* -->

1. [VS Code Official Download Page](https://code.visualstudio.com/Download)  
   Contains download links for both User and System installers.

2. [VS Code Alternate Downloads](https://code.visualstudio.com/#alt-downloads)  
   Explicit options for User Installer, System Installer, and ZIP builds.

3. [VS Code Documentation - Setup](https://code.visualstudio.com/docs/setup/setup-overview)  
   Overview of setup options and behavior across platforms.

4. [VS Code FAQ - Updates](https://code.visualstudio.com/docs/supporting/faq#_updates)  
   Discusses why updates may not work in certain contexts.

5. [Windows AppData Reference](https://learn.microsoft.com/en-us/windows/win32/shell/knownfolderid)  
   Explains special folders like AppData where VS Code stores config files.

6. [Windows Firewall Documentation](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security)  
   Details how application-level firewall rules behave and how to configure them.

7. [PowerShell Get-Command](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-command)  
   Microsoft Docs reference for inspecting how commands are resolved in PowerShell.

