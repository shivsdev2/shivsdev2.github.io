# How I Switched from `wpa_supplicant` to `iwd`

### 1. Install iwd

Install `iwd` using DNF:

```bash
sudo dnf install iwd
```

### 2. Disable wpa_supplicant

Since we will be using `iwd` instead, disable and stop `wpa_supplicant`:

```bash
sudo systemctl disable --now wpa_supplicant
```

### 3. Start the iwd daemon

Start the `iwd` service:

```bash
sudo systemctl start iwd.service
```

### 4. Configure NetworkManager to use iwd

Change the NetworkManager configuration to use `iwd` as its Wi-Fi backend.

Open the configuration file:

```bash
sudo nano /etc/NetworkManager/conf.d/iwd.conf
```

Add the following lines:

```ini
[device]
wifi.backend=iwd
```

Save the file and exit.

### 5. Restart NetworkManager and reboot

Apply the changes by restarting NetworkManager:

```bash
sudo systemctl restart NetworkManager
```

Finally, reboot the system:

```bash
sudo reboot
```

### 6. Recovering saved Wi-Fi passwords

The annoying part of this switch was having to manually enter the Wi-Fi passwords again.
NetworkManager had the existing connection profiles, and I was able to retrieve the saved SSIDs and passwords by running:

```bash
sudo grep -H -E '^(ssid|psk)=' /etc/NetworkManager/system-connections/*.nmconnection
```

This displays the saved Wi-Fi network names (`ssid`) and passwords (`psk`).

Be aware that this command displays Wi-Fi passwords in plain text! Be careful not to share its output or expose it in logs.

