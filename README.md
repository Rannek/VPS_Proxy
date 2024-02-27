
# SSH SOCKS Proxy Setup with Firefox Configuration Guide

This guide will walk you through setting up an SSH SOCKS proxy for secure browsing and configuring Firefox to use this proxy.

## Setting Up the SSH SOCKS Proxy

### Prerequisites

- SSH access to a remote server with your username and its IP address or hostname.
- SSH client installed on your local machine.


# Required SSH Server Configuration for SOCKS Proxy

To ensure the SOCKS proxy functionality works correctly, you may need to verify or adjust certain settings in your SSH server's configuration file (`sshd_config`). Here are the recommended settings:

## Editing the `sshd_config` File

Locate your SSH daemon configuration file, usually found at `/etc/ssh/sshd_config`, and open it with your preferred text editor (you will need root or sudo privileges).

### Ensure TCP Forwarding is Allowed

Look for the following line:

```
AllowTcpForwarding yes
```

If it's not present or commented out (with a `#` at the beginning of the line), add or uncomment it. This setting allows the forwarding of TCP connections, which is necessary for the SOCKS proxy to function.

### Compression

To take advantage of the `-C` option for compression in your SSH command, ensure compression is allowed on the server. Find or add this line:

```
Compression yes
PermitTunnel yes
```

### Restart the SSH Service

After making changes, save the file and restart the SSH service to apply them. The command to restart `sshd` depends on your system; here are two common ones:

For systems using systemd:

```sh
sudo systemctl restart sshd
```

For systems using init:

```sh
sudo service sshd restart
```

### Step 1: Establish the SSH SOCKS Proxy Connection

Open a terminal on your local machine and run the following command:

```sh
ssh -C -D 1080 your_username@remote_server_ip
```

Replace `your_username` with your actual username on the remote server, and `remote_server_ip` with the server's IP address or hostname.

- `-C` enables compression.
- `-D 1080` specifies the local port for the SOCKS proxy.

### Step 2: Keep the Connection Active

Keep the terminal open to maintain the SSH SOCKS proxy connection. You can use tools like `screen` or `tmux` to manage background sessions.

## Configuring Firefox to Use the SOCKS Proxy

### Step 1: Open Firefox Preferences

Open Firefox, go to the menu (☰), and select `Preferences` (or `Options` on some platforms).

### Step 2: Navigate to Network Settings

Scroll down to the bottom of the page and click on `Settings` under the `Network Settings` section.

### Step 3: Configure Proxy

In the `Connection Settings` window, select the `Manual proxy configuration` option. Enter `127.0.0.1` in the `SOCKS Host` field and `1080` in the corresponding port field. Make sure to select SOCKS v5.

Optionally, check the option `Proxy DNS when using SOCKS v5` to ensure all DNS requests are also tunneled through the SSH connection.

### Step 4: Save and Close

Click `OK` to save your settings and close the preferences tab.

## Verifying the Configuration

Visit a website like http://whatismyipaddress.com/ to verify that your IP address matches the remote server's IP. This confirms that your browsing traffic is being tunneled through the SSH SOCKS proxy.

## Troubleshooting

If you cannot connect to the internet after configuring the proxy, double-check your Firefox proxy settings and ensure your SSH connection is active.

## Conclusion

You've now set up an SSH SOCKS proxy and configured Firefox to use it for secure browsing. This setup can enhance your privacy and security on the internet by encrypting your browsing traffic.
