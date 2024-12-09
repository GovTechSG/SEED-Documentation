# Troubleshooting issues

This guide provides solutions to common problems for SEED. Follow the steps below to troubleshoot and resolve the problems you are experiencing.

## Resolving *Cloudflare CF_DNS_Lookup_Failure* error on macOS 15

![Firewall settings](/images/cferror.png)

If you encounter the `CF_DNS_Lookup_Failure` error while using Cloudflare WARP on macOS 15, follow these steps to resolve the issue:


### Upgrade macOS
Cloudflare recommends upgrading to **macOS 15.1 or later**, as macOS 15.1 resolves several issues that could cause the WARP client to misbehave.

- Open **System Preferences**.
- Select **Software Update**.
- Follow the on-screen instructions to update to **macOS 15.1 or later**.

### Enable firewall settings

Ensure your firewall settings are properly configured to allow Cloudflare WARP to function.

- Open **System Preferences**.
- Navigate to **Settings > Network > Firewall**.
- Turn on the firewall.

![Firewall settings](/images/firewall-macos15.png)


### Disable *block all incoming connections*
Adjust your firewall settings to allow incoming connections.

- Go to **Settings > Network > Firewall > Options**.
- Ensure **"Block all incoming connections"** is **unchecked**.


### Allow incoming connections for Cloudflare WARP
Enable incoming connections explicitly for the following applications:

1. **CloudflareWARP** located at:  
   `/Applications/CloudflareWARP.app/Contents/Resources/CloudflareWARP`
2. **Cloudflare WARP** with the bundle ID:  
   `com.cloudflare.1dot1dot1dot1dot1.macos`

![Allow incoming connections](/images/allow-warp.png)

After completing these steps, the `CF_DNS_Lookup_Failure` error should no longer occur.


## Troubleshooting access issues for SGTS Resources

If you encounter the screen below:

![Error Screen](/images/cf-i1.png)

Follow these steps to diagnose the issue:

1. **Check the SEED Dashboard**: Start by investigating the reason why access to certain resources is restricted. Ensure the following:

   - Your Cloudflare WARP client is connected and enrolled in the `gccgovsg` organisation.
   - The Tanium Client is installed and running on your device.

Once these conditions are met, you can access the SEED Dashboard on your GMD to further diagnose the issue with your device.

### Possible reasons for blocking

Access to SGTS resources may be blocked for the following reasons:

| **Issue** | **Description** | **Category** |
|---|---|---|
| Malware on the device | The device is infected with malware or exhibits suspicious behavior, causing it to be classified as high-risk and blocking access to SGTS. | Security |
| Incorrect Cloudflare configuration | The Cloudflare WARP client is either stopped or not enrolled in the `gccgovsg` organization, marking the device as misconfigured. | Configuration |
| Incorrect Intune configuration | Device not enrolled in the correct tenants (`SG Govt M365` or `TECHPASS`), or unable to sync with Intune, blocking access to resources. | Configuration |
| Tanium Client cannot communicate | The Tanium Client is unable to communicate with our servers, leading to blocked access due to lack of necessary status updates.| Configuration |


## Tanium issues

<!-- tabs:start -->

### **Windows**

**Check for Tanium Client installation**

1. Click the Start icon on the taskbar.
2. Go to *Settings* > *Apps* and search for Tanium Client.
   
![tanium](/images/tanium-client-win.png)

3. If you are unable to find it, try to resync your Intune to install the Tanium Client on your device.

### **macOS**

**Check for Tanium Client installation**

1. Open *Terminal*.
2. Run the following command:
   ```sudo ls /Library/Tanium/TaniumClient```
3. Enter your macOS password when prompted.
4. If you see confirmation, as shown in the image below, Tanium Client is installed on your device.
![tanium-client](/images/tanium-client-mac.png)
5. If you are unable to find it, log into Company Portal to install the Tanium Client on your device. Otherwise, you might need to re-onboard your device following the steps [here](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/onboard-device/seed-prerequisites).


<!-- tabs:end -->

## Other Tanium issues

If your device remains blocked after checking all other settings, please try to connect using a mobile hotspot and wait for 15-20 minutes for the Tanium server to find your device.

If your device is unblocked after connecting via mobile hotspot, it is likely that the firewall of the previous network you are connecting to is blocking the Tanium IPs. Whitelist the Tanium IPs to resolve this issue. The IP addresses can be found at this [link](https://docs.developer.tech.gov.sg/docs/tanium-ip/README) (accessible via TechPass login).

## Receiving notifications on desktop and email for misconfigurations on SEED device

<!-- tabs:start -->

### **Windows**

#### Cloudflare

**Ensure that Cloudflare service is installed and running**

1. Search for *Add or Remove Programs* in the search bar.
2. Ensure that Cloudflare Warp shows up as an installed program.

![cf](/images/cf-installed.png)

3. Search for *Run* in the search bar and run *services.msc*.
4. Ensure that Cloudflare WARP is a running service.

![cf run](/images/cf-running.png)

5. If Cloudflare WARP is not installed or not running, refer to [troubleshooting Cloudflare issues](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/troubleshooting-issues).

**Ensure that Cloudflare is authenticated and verified**

1. Open the taskbar, right-click on the Cloudflare Warp icon, and then click on *Preference*.
2. Navigate to *Account* and ensure that you are authenticated with the team name *gccgovsg*.

![acc-cf](/images/acc-cf.png)

3. If Cloudflare WARP is not authenticated to *gccgovsg*, refer to [troubleshooting issues](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/troubleshooting-issues).

**For any other Cloudflare issues, refer to the Cloudflare troubleshooting information in the relevant section on this page.**

#### Intune

**Ensure that Intune is enrolled with the valid tenant**

1. Search for *Access work or school* in the search bar.
2. Ensure that you have an account added with a *techpass.gov.sg* domain (for vendors) or a *<agency_name>.gov.sg* domain (for all Public officers).

![work](/images/work-school.png)

3. Click the account > *Info*.
4. Ensure that the top of the page shows either *Managed by SG Govt M365* for public officers or *Managed by TechPass* for vendors.

![cf](/images/managed-by.png)

5. If there is no enrollment, refer to the following links for help on how to enroll your device to Intune:
   - [Public officer](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/onboard-device/public-officer)
   - [Vendor](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/onboard-device/vendor)

#### Defender

**Ensure that the Defender service is running**

1. Search for *Run* in the search bar and type *services.msc*.
2. Ensure that *Microsoft Defender Antivirus Service* and *Microsoft Defender Core Service* are running.

![defender run](/images/defender-run.png)

3. If the services are not running, reboot your system and verify again, or try to start them manually by right-clicking and click *Start*.
4. If your services are unable to start after multiple attempts, raise a [service request](http://go.gov.sg/seed-techpass-support)

**Ensure that real-time protection and cloud protection are turned on**

1. Search for *Windows Security* in the search bar.
2. Navigate to *Virus & threat protection* > *Manage Settings* under *Virus & threat protection settings*.
3. Ensure that *Real-time protection* and *Cloud-delivered protection* are enabled.

![rt](/images/realtime-protection.png)

4. If they cannot be enabled, navigate to the previous page and click *Check for Updates* under *Virus & threat protection Updates*, and install the updates, if any.
5. If it is still not enabled, raise a [service request](http://go.gov.sg/seed-techpass-support), and state *Disabled Real-Time protection and Cloud-delivered protection* in the description.

**Ensure that Defender has a valid tenant**

1. Go to the Start menu and enter Powershell.
2. Right-click on the search result for PowerShell and select Run as Administrator
3. On Powershell, run the following command:
```
$reg64 = [Microsoft.Win32.RegistryKey]::OpenBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine, [Microsoft.Win32.RegistryView]::Registry64)
$OrgID =  $reg64.OpenSubKey("SOFTWARE\MICROSOFT\Windows Advanced Threat Protection\Status").GetValue("OrgID")
echo $OrgID
```
4. Take note of the value displayed for OrgID.
5. Ensure that the value is one of the following:
   - Public officers: faa36a5e-2da6-4225-8e27-226177c801a0
   - Vendors: 49237d71-42ac-425a-a803-881b92cc18ce
6. If the values are different, check with your IT administrator to identify which Defender tenant you are currently under and offboard from that tenant before attempting to re-onboard.

### **macOS**

#### Cloudflare

**Ensure that Cloudflare service is installed and running**

1. Access the *Application* folder.
2. Ensure that Cloudflare WARP is appearing as an installed program.

![mac-cf](/images/mac-cf.png)

3. Open *Terminal*.
4. Type `sudo launchctl list | grep -i cloudflare` and ensure the service is running.
5. If Cloudflare WARP is not installed or not running, refer to the Cloudflare troubleshooting information in the relevant section on this page.

> **Note**: Spotlight is used to detect the existence of applications. If you have disabled Spotlight, we would not be able to detect Cloudflare WARP on your device.

**Ensure that Cloudflare is authenticated and verified**

1. Click the *Cloudflare WARP* icon on the taskbar > *Preference*.
2. Navigate to *Account* and ensure that you are authenticated with the team name *gccgovsg*.

![mac-gcc](/images/mac-gcc.png)

3. If Cloudflare WARP is not authenticated to *gccgovsg*, refer to [troubleshooting issues](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/troubleshooting-issues).

#### Intune

**Ensure that Intune is enrolled with the valid tenant**

1. Log into Company Portal using your `<agency_name>.gov.sg` or `techpass.gov.sg` username.
2. Ensure that your device is shown as **managed**.
3. Ensure that the top right-hand corner shows *SG Govt M365* or *TechPass* to show it is managed by the correct tenant.

![mac-365](/images/mac-365.png)

4. If there is no enrollment, refer to the following links for help on how to enroll to Intune:

   - [Public officer](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/onboard-device/public-officer)
   - [Vendor](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/onboard-device/vendor)


> **Note**: Perform a sync to check that the device is able to communicate with Intune.

#### Defender

**Ensure that the Defender service is installed**

1. Access the *Application* folder.

![mac-app](/images/mac-app.png)

**Ensure that the Defender service is running and is healthy**

1. Open *Terminal*.
2. Run `ps aux | grep -i "Defender"`.
3. Ensure you see the following output:

![defender-terminal](/images/defender-terminal.png)

**If the service is not running, refer to the following steps**:

1. Open *Terminal*.
2. Run the following command to start the services:
   - `sudo launchctl kickstart -k system/com.microsoft.wdav`
   - `sudo launchctl kickstart -k system/com.microsoft.wdav.enterprise`
3. Run `ps aux | grep -i "Defender"` to verify if the services are running now.
4. If the issue persists, raise a [service request](http://go.gov.sg/seed-techpass-support)

**Ensure that Defender has real-time protection enabled**

1. Open *Terminal*.
2. Run `mdatp health --field real_time_protection_enabled`.
3. Ensure that the value is *true*.

**Ensure that Defender is cloud enabled**

1. Open *Terminal*.
2. Run `mdatp health --field cloud_enabled`.
3. Ensure that the value is *true*.

![mac-virus](/images/mac-virus.png)

**Ensure that Defender has a valid tenant**

1. Open *Terminal*.
2. Run `mdatp health --field org_id`.
3. Ensure that the value is one of the following:
   - Public officers: faa36a5e-2da6-4225-8e27-226177c801a0
   - Vendors: 49237d71-42ac-425a-a803-881b92cc18ce
4. If the values are different, please check with your IT administrator to identify which Defender tenant you are currently under and offboard from that tenant before attempting to re-onboard.

**Ensure that Defender definitions are updated**

1. Open Microsoft Defender.
2. Under *Virus & threat protection updates*, click *Check for updates*.

![mac-check](/images/mac-check.png)

3. Click *Help* > *Check for product updates*.
4. Click *Update All*.

![mac-virus](/images/mac-update-all.png)

<!-- tabs:end -->


## Experiencing the *You cannot access this right now* error 

![error](/images/troubleshoot-issue.png)

When using SGTS products with Cloudflare WARP turned off, you might encounter an error message saying, *That account does not have access*.

### Solution

- Turn on Cloudflare WARP Client and access the application. 

If you are facing an issue with your Cloudflare WARP, please follow the solutions in this page. Alternatively, [raise a service request](https://go.gov.sg/seed-techpass-support) for assistance.

## Cloudflare WARP is in *Connecting* status

If your Cloudflare WARP is stuck in the connecting status, please follow these steps to resolve the issue:

1. Click the **Start** icon in the taskbar.

2. Navigate to **Settings** > **Apps**.

3. Search for **Cloudflare WARP** and select **Uninstall**.

After uninstalling, proceed to [download Cloudflare WARP](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/download-warp/)

For a smooth experience, download the following versions:

- **Windows**: Version 2024.3.409.0
- **macOS**: Version 2024.3.444.0

Once downloaded, follow these steps:

1. Click the **gear** icon > **Preferences** > **Account**.

   ![gear](/images/disconnected-cf.png)

2. Log in with Cloudflare for Teams.
3. Enter **gccgovsg** in the organisation name field.
4. 
   ![gear](/images/gcc-org.png)
   
6. Test using incognito mode using Google Chrome or Microsoft Edge browser and test using your personal hotspot or home Wi-Fi.

Ensure to re-authenticate your Cloudflare WARP client with the following steps:

1. Clear your browsing history/cache on Chrome.

2. Click the **Cloudflare WARP** icon.
   
   - Click the **gear** icon.
   - Navigate to **Preferences** > **Account**.
   - Click **Re-authenticate with Cloudflare zero trust**.

3. Reboot your machine.


## Experiencing the *Account does not have access* error

When using SGTS products with Cloudflare WARP, you might encounter an error message saying, *That account does not have access*.

### Solution

1.  First, check the following:

    - Have you received the *successfully onboarded* email from SEED?
    - Are you using one of the supported browsers?
    - Is your Cloudflare WARP client connected and up to date?
    - Open Cloudflare WARP **Settings** and ensure "Gateway with WARP" is selected.
    - For Windows users, check if Tanium is listed in the **Start** menu.
    - For macOS users, look for Tanium in **Finder** > **Applications**.
    - Ensure your device's operating system is up to date.
    - Make sure Defender is updated and running.
    - Check if your TechPass account has the necessary permissions for GCC 2.0 CMP or a specific SGTS service.

> **Note**:
>- SEED does not support running other VPN clients alongside Cloudflare WARP.
>- It is recommended not to use WARP and a VPN simultaneously.

2. If you are running a VPN client along with WARP, ensure that the VPN configuration doesn't route all traffic and DNS queries to the VPN server.

3. If the issues persist, [generate a diagnostic report](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/generate-diagnostic-files) and upload it to the [service request](https://go.gov.sg/seed-techpass-support).


## Connectivity issues for macOS and windows WARP users

Cloudflare has reported connectivity problems for users with macOS and Windows WARP. Users may experience intermittent connectivity issues while trying to access websites.


### Solutions

#### Workaround: Uninstall and reinstall Cloudflare WARP

<!-- tabs:start -->

#### **macOS**

1. To uninstall the existing WARP client, open the ***Terminal*** app and run the following command.

  ```
  sudo /bin/sh /Applications/Cloudflare\ WARP.app/Contents/Resources/uninstall.sh
  ```
2. Enter your macOS password when prompted. You will be prompted to confirm the uninstallation.

  ```Do you want to uninstall Cloudflare WARP app? Enter Y to proceed or N to exit.```

3. Enter `Y`. When WARP is successfully uninstalled, the message ```Finished uninstallation!``` is displayed.

4. Proceed to [download Cloudflare WARP](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/download-warp/).
   - **macOS**: Version 2024.3.444.0 

#### **Windows**

1. To uninstall the existing WARP client, click the **Start** icon on the taskbar.
2. Go to **Settings** > **Apps** and search for **Cloudflare WARP**.
3. Choose Cloudflare WARP and click **Uninstall**.
4. Proceed to [download Cloudflare WARP](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/download-warp/).
   - **Windows**: Version 2024.3.409.0

<!-- tabs:end -->

Once downloaded, follow the steps below:

1. Click the **gear** icon > **Preferences** > **Account**.

   ![gear](/images/disconnected-cf.png)

2. Log in with Cloudflare for Teams.
3. Enter **gccgovsg** in the organisation name field.

   ![gear](/images/gcc-org.png)
   
5. Test using incognito mode using Google Chrome or Microsoft Edge browser and test using your personal hotspot or home Wi-Fi.

Ensure to re-authenticate your Cloudflare WARP client with the following steps:

1. Clear your browsing history/cache on Chrome.

2. Click the **Cloudflare WARP** icon.
   
   - Click the **gear** icon.
   - Navigate to **Preferences** > **Account**.
   - Click **Re-authenticate with Cloudflare zero trust**.

3. Reboot your machine.


