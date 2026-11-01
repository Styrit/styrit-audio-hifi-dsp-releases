# Windows setup for the optional Styrit test driver

This guide covers Windows 10 and Windows 11 x64. The optional Styrit Virtual Audio
Driver is test signed; it is not Microsoft certified. Prepare Windows yourself
before selecting the driver component. Setup and the app never change boot or
security settings and never restart Windows automatically.

## Choose an installation mode

- **App only** works without preparing Windows. It does not install drivers or
  certificates. Other audio sources remain available, and an existing compatible
  Styrit driver can still be used.
- **App with Styrit driver** installs the app for your user and the optional driver
  for the whole computer. The driver step requires administrator permission and
  the prerequisites below.

Release 0.1.0 is an app-only installer and requires a separately installed driver
for the Styrit source. This guide also describes the optional driver component
planned for 0.2.0; it does not mean that component has already been released.

## Prepare Windows

1. **Make sure your BitLocker recovery key is accessible.** Firmware and boot
   changes can trigger recovery. Follow Microsoft's
   [BitLocker recovery guidance](https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/bitlocker-key-management-faq)
   before making changes. On a managed computer, contact your administrator.
2. **Disable Secure Boot in firmware.** Follow your computer manufacturer's
   instructions. If the system does not support Secure Boot, this step does not apply.
3. Open **Command Prompt as administrator** and run:

   ```cmd
   bcdedit /set testsigning on
   ```

4. **Restart Windows yourself.** The command changes a boot option; it does not
   enable test signing in the session that is already running. See Microsoft's
   [test-signing instructions](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/the-testsigning-boot-configuration-option).
5. Run the complete Styrit setup again. It checks the active Windows session
   before offering driver installation.

The driver step is available only when test signing is active, Secure Boot is off
or unsupported, and the memory-integrity state is supported by the package.
Unknown or contradictory results block the driver step; **App only** remains available.

## Memory integrity (HVCI)

Do not disable memory integrity as a blanket preparation step. Installation with
HVCI enabled is offered only when the exact driver package has passed the relevant
internal tests. If that package has not been approved for HVCI, use **App only**.

Other Windows or organization policies can still prevent a driver from loading.
The installer does not disable those policies. Report the error and review the
Windows Code Integrity event log with your administrator if needed.

## Install the driver and certificate

Run the complete Styrit setup and select **App with Styrit driver**. To install
or repair the driver later, run that setup again. The app does not contain driver
installation or repair actions. Close the app before running setup manually.

Approve the administrator prompt only if you intended to install the driver.
Before importing the dedicated Styrit test certificate, the helper displays its
SHA-256 fingerprint and explains its purpose. Compare it with the fingerprint
published with the driver package before consenting. Only the package's fixed
public certificate is added to the computer's Trusted Root and Trusted Publishers
stores. Declining certificate trust or cancelling UAC does not complete the
selected driver installation. A different certificate requires consent again.

If Windows reports that a restart is required, restart it yourself before using
the Styrit source. Other player processes are not forcibly closed.

## Updates and removal

The installer remembers your component choice for your user. An update without a
saved choice, including an upgrade from 0.1.0, does not introduce a driver or
certificate installation automatically. A compatible driver is reused without a
new administrator prompt. If Windows no longer meets the prerequisites, the app
can update while the optional driver step remains deferred. If you then try to
start the Styrit source, the app explains why it cannot start. Prepare Windows
using this guide and run the complete setup again to finish the driver step.

Removing the app leaves the shared driver and test certificate installed. Use the
separate **Styrit Virtual Audio Driver** uninstaller to remove the Styrit driver.
The certificate is not removed automatically because other installations may
still use it. Certificate removal or reverting Windows boot settings is a
separate administrator decision.
