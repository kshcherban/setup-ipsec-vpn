# Configure IPsec/XAuth VPN Clients

*Read this in other languages: [English](clients-xauth.md), [简体中文](clients-xauth-zh.md).*

*Note: You may also connect using [IPsec/L2TP mode](clients.md), or set up [IKEv2](ikev2-howto.md).*

After <a href="https://github.com/hwdsl2/setup-ipsec-vpn" target="_blank">setting up your own VPN server</a>, follow these steps to configure your devices. IPsec/XAuth ("Cisco IPsec") is natively supported by Android, iOS and OS X. There is no additional software to install. Windows users can use the free <a href="https://www.shrew.net/download/vpn" target="_blank">Shrew Soft client</a>. In case you are unable to connect, first check to make sure the VPN credentials were entered correctly.

IPsec/XAuth mode is also called "Cisco IPsec". This mode is generally faster than IPsec/L2TP with less overhead.

---
* Platforms
  * [Windows](#windows)
  * [OS X (macOS)](#os-x)
  * [Android](#android)
  * [iOS (iPhone/iPad)](#ios)
  * [Linux](#linux)

## Windows

**Note:** You may also connect using [IPsec/L2TP mode](clients.md). No additional software is required.

1. Download and install the free <a href="https://www.shrew.net/download/vpn" target="_blank">Shrew Soft VPN client</a>.   
   **Note:** This VPN client supports Windows 2K/XP/Vista/7/8.
1. Click Start Menu -> All Programs -> ShrewSoft VPN Client -> VPN Access Manager
1. Click the **Add (+)** button on toolbar.
1. Enter `Your VPN Server IP` in the **Host Name or IP Address** field.
1. Click the **Authentication** tab. Select **Mutual PSK + XAuth** from the **Authentication Method** drop-down menu.
1. Under the **Local Identity** sub-tab, select **IP Address** from the **Identification Type** drop-down menu.
1. Click the **Credentials** sub-tab. Enter `Your VPN IPsec PSK` in the **Pre Shared Key** field.
1. Click the **Phase 1** tab. Select **main** from the **Exchange Type** drop-down menu.
1. Click the **Phase 2** tab. Select **sha1** from the **HMAC Algorithm** drop-down menu.
1. Click **Save** to save the VPN connection details.
1. Select the new VPN connection. Click the **Connect** button on toolbar.
1. Enter `Your VPN Username` in the **Username** field.
1. Enter `Your VPN Password` in the **Password** field.
1. Click **Connect**.

Once connected, you will see **tunnel enabled** in the VPN Connect status window. Click the "Network" tab, and confirm that **Established - 1** is displayed under "Security Associations". You can verify that your traffic is being routed properly by <a href="https://www.google.com/search?q=my+ip" target="_blank">looking up your IP address on Google</a>. It should say "Your public IP address is `Your VPN Server IP`".

If you get an error when trying to connect, see <a href="clients.md#troubleshooting" target="_blank">Troubleshooting</a>.

## OS X

1. Open System Preferences and go to the Network section.
1. Click the **+** button in the lower-left corner of the window.
1. Select **VPN** from the **Interface** drop-down menu.
1. Select **Cisco IPSec** from the **VPN Type** drop-down menu.
1. Enter anything you like for the **Service Name**.
1. Click **Create**.
1. Enter `Your VPN Server IP` for the **Server Address**.
1. Enter `Your VPN Username` for the **Account Name**.
1. Enter `Your VPN Password` for the **Password**.
1. Click the **Authentication Settings** button.
1. In the **Machine Authentication** section, select the **Shared Secret** radio button and enter `Your VPN IPsec PSK`.
1. Leave the **Group Name** field blank.
1. Click **OK**.
1. Check the **Show VPN status in menu bar** checkbox.
1. Click **Apply** to save the VPN connection information.

To connect to the VPN: Use the menu bar icon, or go to the Network section of System Preferences, select the VPN and choose **Connect**. You can verify that your traffic is being routed properly by <a href="https://www.google.com/search?q=my+ip" target="_blank">looking up your IP address on Google</a>. It should say "Your public IP address is `Your VPN Server IP`".

## Android

1. Launch the **Settings** application.
1. Tap **More...** in the **Wireless & Networks** section.
1. Tap **VPN**.
1. Tap **Add VPN Profile** or the **+** icon at top-right of screen.
1. Enter anything you like in the **Name** field.
1. Select **IPSec Xauth PSK** in the **Type** drop-down menu.
1. Enter `Your VPN Server IP` in the **Server address** field.
1. Leave the **IPSec identifier** field blank.
1. Enter `Your VPN IPsec PSK` in the **IPSec pre-shared key** field.
1. Tap **Save**.
1. Tap the new VPN connection.
1. Enter `Your VPN Username` in the **Username** field.
1. Enter `Your VPN Password` in the **Password** field.
1. Check the **Save account information** checkbox.
1. Tap **Connect**.

Once connected, you will see a VPN icon in the notification bar. You can verify that your traffic is being routed properly by <a href="https://www.google.com/search?q=my+ip" target="_blank">looking up your IP address on Google</a>. It should say "Your public IP address is `Your VPN Server IP`".

If you get an error when trying to connect, see <a href="clients.md#troubleshooting" target="_blank">Troubleshooting</a>.

## iOS

1. Go to Settings -> General -> VPN.
1. Tap **Add VPN Configuration...**.
1. Tap **Type**. Select **IPSec** and go back.
1. Tap **Description** and enter anything you like.
1. Tap **Server** and enter `Your VPN Server IP`.
1. Tap **Account** and enter `Your VPN Username`.
1. Tap **Password** and enter `Your VPN Password`.
1. Leave the **Group Name** field blank.
1. Tap **Secret** and enter `Your VPN IPsec PSK`.
1. Tap **Done**.
1. Slide the **VPN** switch ON.

Once connected, you will see a VPN icon in the status bar. You can verify that your traffic is being routed properly by <a href="https://www.google.com/search?q=my+ip" target="_blank">looking up your IP address on Google</a>. It should say "Your public IP address is `Your VPN Server IP`".

## Linux

Example is based on Ubuntu 18.04 but may be adapted to any linux flavour.

For unknown reason XAuth password file authentication doesn't work with `vpnc`, so script uses `pam` authentication.
This requires you to connect with existing system users/passwords.
Please check [libreswan documentation](https://libreswan.org/wiki/VPN_server_for_remote_clients_using_IKEv1_XAUTH_with_PSK#Aggressive_Mode) to get acquainted with PAM auth and aggressive connection mode that `vpnc` can only use.
The easiest way is to create some user with no shell access and use that for VPN.
On the VPN server execute:
```bash
sudo useradd -s /usr/sbin/nologin -r vpnuser
```
This will create system user `vpnuser` with no shell access and no home directory.
And now set this user some password.

```bash
sudo passwd vpnuser
```

1. Install `vpnc` and corresponding network manager plugin:
    ```bash
    sudo apt-get install network-manager-vpnc network-manager-vpnc-gnome vpnc
    ```
1. Add new VPN connection of type **Cisco Compatible VPN (vpnc)** to network connections.

1. If you haven't set `VPN_GROUP` variable during setup, then set **Group Name** to `group`. All other parameters should be set as following:  
**Gateway** - `Your VPN Server IP`  
**User name** - `Your VPN Username of system user`  
**User password** - `Your VPN Password of system user`  
**Group password** - `Your VPN IPsec PSK`
1. Save and connect to your new VPN.

Alternatively you can connect with command line: `vpnc --gateway <your server ip> --id <your vpn group> --username <your vpn username>`

Please also check https://help.ubuntu.com/community/VPNClient#VPNC for details how VPNC can be configured in Ubuntu.


## Credits

This document was adapted from the <a href="https://github.com/StreisandEffect/streisand" target="_blank">Streisand</a> project, maintained by Joshua Lund and contributors.

## License

Note: This license applies to this document only.

Copyright (C) 2016-2018 Lin Song   
Based on <a href="https://github.com/StreisandEffect/streisand/blob/6aa6b6b2735dd829ca8c417d72eb2768a89b6639/playbooks/roles/l2tp-ipsec/templates/instructions.md.j2" target="_blank">the work of Joshua Lund</a> (Copyright 2014-2016)

This program is free software: you can redistribute it and/or modify it under the terms of the <a href="https://www.gnu.org/licenses/gpl.html" target="_blank">GNU General Public License</a> as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
