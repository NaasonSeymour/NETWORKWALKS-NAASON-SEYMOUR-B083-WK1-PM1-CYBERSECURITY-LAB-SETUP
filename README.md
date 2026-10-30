# WK1-PM1 Cybersecurity Lab Setup

**Participant:** Naason Seymour  
**Batch:** B083  
**Week / project:** WK1-PM1

This project documents one Kali Linux virtual machine on a Windows PC for an owned cybersecurity training lab.

**Status:** The required lab configuration is complete. Kali boots, uses the required NAT Network and static IPv4 address, loads a live website, exchanges clipboard text in both directions, and reads/writes the Windows Downloads share. A clean powered-off baseline snapshot was saved, restored, and booted successfully. Course publication and submission are pending.

## Host and software

| Component | Configuration |
|---|---|
| Host | Windows 11 Pro, 64-bit |
| Processor | AMD Ryzen 7 3700X, hardware virtualization enabled |
| Host memory | Approximately 16 GB |
| VirtualBox | 7.2.22 r175245, official Oracle Windows installer |
| Kali | Official prebuilt VirtualBox image, Kali Linux 2026.3 amd64 |
| VM name | Kali - WK1-PM1 |
| VM resources | 4 GB RAM, 2 CPU cores, VMSVGA, 128 MB video memory |

The Oracle installer checksum matched the official SHA-256 manifest and had a valid Oracle America digital signature. The Kali archive checksum matched the value published on the official Kali download page.

## Network and integration

| Setting | Value |
|---|---|
| VirtualBox network | WK1-PM1-NAT |
| Attachment type | NAT Network |
| Network prefix | 10.0.0.0/24 |
| Kali IPv4 address | 10.0.0.2/24, manual |
| Gateway | 10.0.0.1 |
| DNS | 10.0.0.1, course-documented alternate |
| DHCP | Disabled for this single-VM static-address lab |
| Promiscuous mode | Allow All, as demonstrated by the course |
| Shared clipboard | Bidirectional |
| Drag and drop | Bidirectional |
| Shared folder | Host Windows Downloads folder, share name downloads |
| Guest mount | /media/sf_downloads, permanent and automatically mounted |

Only one VM is registered in this new lab. No second VM is using the assigned lab address.

## Steps performed and evidence

1. Installed the verified official VirtualBox Windows application and required virtualization driver.
2. Created and enabled the NAT Network with the required prefix.
   ![VirtualBox NAT Network](screenshots/01-VirtualBox-NAT-Network-10.0.0.0-24.jpg)
3. Extracted the verified official Kali prebuilt archive and registered its .vbox file.
4. Assigned the VM to the lab NAT Network and set its resources.
   ![Kali Adapter 1 attached to the correct NAT Network](screenshots/02-Kali-VM-Network-Attached-to-NAT-Network.jpg)
5. Enabled bidirectional clipboard and drag and drop and added the permanent Downloads share.
   ![Both bidirectional integration settings in the VM Features tab](screenshots/03-Kali-VM-Shared-Clipboard-and-Drag-and-Drop.jpg)
   Clipboard transfer was tested successfully in both directions; see the [clipboard check](clipboard-verification.json). Drag and drop is enabled in both directions; a file drag-and-drop transfer was not separately tested. The Downloads mapping and automatic mount are also recorded in the [selected VirtualBox configuration report](vm-configuration.txt), with host-identifying values omitted for public sharing.
   ![Permanent Windows Downloads mapping, downloads share name, and automatic mounting](screenshots/04-Shared-Folders-Windows-Downloads-Mapped-to-Kali.jpg)
   A harmless test file created in Windows was read and updated inside Kali, and Windows then read the guest's update. See the [shared-file check](shared-folder-verification.json). The settings screenshot shows the shared-folder mapping, with the host Windows user-profile path redacted for privacy; share name, access, and auto-mount settings remain visible.
6. Booted Kali and configured its NetworkManager profile with a manual address, gateway, and DNS.
   ![Kali manual IPv4 settings](screenshots/05-Kali-IPv4-10.0.0.2-24-Gateway-and-DNS.jpg)
7. Verified the guest address and route, successful gateway replies, DNS resolution, successful HTTPS access to the Kali website, and read/write permissions on the Downloads mount. See [captured verification output](verification.txt).
   ![Live HTTPS website loaded in Firefox inside Kali](screenshots/06-Kali-Desktop-Internet-Browser-Test.jpg)
8. Shut the guest down, saved **Clean Kali - WK1-PM1**, restored that baseline, and booted it again. The static address, DNS, HTTPS access, and Downloads mount passed the [post-restore checks](post-restore-verification.txt). After the logged-in browser, clipboard, and shared-file checks, shut the guest down cleanly again and saved **Clean Kali - WK1-PM1 - Verified** as the final baseline. See the [snapshot record](snapshot.txt).
   ![Saved clean Kali baseline snapshot](screenshots/07-VirtualBox-Clean-Setup-Snapshot.jpg)

The screenshots above are captures of this lab; screenshot 04 has only its host Windows user-profile path redacted for privacy. During the evidence audit, VirtualBox's Settings dialog opened successfully. The adapter, integration, and Shared Folders screenshots now show the exact settings pages requested in the guide. The older summary, separate runtime menus, and mounted-folder screenshots have been preserved locally outside this publication package.

## Evidence audit and required filenames

All eight local setup steps and all seven screenshot categories were checked against the supplied requirements on October 29, 2026. The guide numbers the screenshot categories 01–07, separately from setup steps 01–08. Windows-safe descriptive filenames preserve that evidence numbering; slashes and colons from the guide's descriptions are replaced with hyphens.

| Evidence | Filename in screenshots/ | Result |
|---|---|---|
| 01 — NAT Network prefix | 01-VirtualBox-NAT-Network-10.0.0.0-24.jpg | Captured; 10.0.0.0/24 visible |
| 02 — Kali VM Network settings | 02-Kali-VM-Network-Attached-to-NAT-Network.jpg | Captured; Adapter 1 enabled, NAT Network and WK1-PM1-NAT visible |
| 03 — Clipboard and drag and drop | 03-Kali-VM-Shared-Clipboard-and-Drag-and-Drop.jpg | Captured together; both Bidirectional |
| 04 — Host Downloads mapping | 04-Shared-Folders-Windows-Downloads-Mapped-to-Kali.jpg | Captured; downloads under Machine Folders, host Downloads path, Auto Mount Yes |
| 05 — Kali manual IPv4 settings | 05-Kali-IPv4-10.0.0.2-24-Gateway-and-DNS.jpg | Captured; manual address, prefix, gateway, alternate DNS visible |
| 06 — Kali browser Internet test | 06-Kali-Desktop-Internet-Browser-Test.jpg | Captured; live HTTPS Kali website loaded |
| 07 — Clean snapshot | 07-VirtualBox-Clean-Setup-Snapshot.jpg | Captured; final verified baseline and powered-off VM visible |

VirtualBox installation and Kali registration are also visible in the Manager/snapshot screenshot. Only one VM is registered, so there is no second lab VM using 10.0.0.2. Clipboard and actual shared-file checks passed in both directions. Drag-and-drop configuration is verified; a separate file-drag test was not required by this checklist and has not been claimed.

## Troubleshooting experienced

The initial DNS setting, 8.8.8.8, timed out. Kali could already reach both the lab gateway and an external numeric address, which narrowed the issue to name resolution. A direct DNS query to the NAT gateway at 10.0.0.1 succeeded. The connection was changed to this course-documented alternate DNS and reactivated. Kali then resolved the website and received HTTP/2 200 over HTTPS.

The course's conditional duplicate-address-detection workaround was not needed.

The VirtualBox Manager settings dialog stalled during the initial setup. Configuration was applied and verified using VirtualBox's command-line interface. A missing host USB component was installed and its stale background service refreshed, clearing the device-discovery warning. On the later evidence audit, the Settings dialog opened successfully in Basic mode, allowing exact adapter, integration, and shared-folder screenshots to be captured. No specific cause for the earlier stall has been established. The recorded guest boot, network, share, and snapshot checks passed.

## Course submission status

**Awaiting participant review and official submission. Publishing status is not recorded in this repository.**

- [ ] Publish this README and the completed real screenshot set to GitHub or Google Drive.
- [ ] Publish the LinkedIn account of the completed work, tag the specified course accounts, and include #Networkwalks. Keep IP addresses and links out of the post body; place the project link in the first comment.
- [ ] Share the actual DNS troubleshooting experience in the instructor's project-video comments and save evidence of the posted comment.
- [ ] Submit the official form with participant details, project link, and LinkedIn post link, then retain confirmation.

The Program Guide lists Friday 11:59 p.m. Dubai time as the deadline (Friday, October 30, 2026, for this week); confirm any instructor updates. The Week 1 video is https://www.youtube.com/watch?v=K6rMQdg10Zk. The final public project/post links and participant account confirmation remain to finalize submission.

## Authorized use

This setup and its checks are restricted to an owned lab environment. No third-party scanning or exploitation was performed.

## Official software sources

- [Oracle VirtualBox downloads](https://www.virtualbox.org/wiki/Downloads)
- [Kali prebuilt virtual machines](https://www.kali.org/get-kali/#kali-virtual-machines)
- [Kali's prebuilt VirtualBox import instructions](https://www.kali.org/docs/virtualization/import-premade-virtualbox/)
