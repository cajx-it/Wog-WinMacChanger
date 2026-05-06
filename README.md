# Wog-WinMacChanger

> A Windows-native MAC address spoofer for external USB WiFi adapters — built for authorized penetration testing and network research.

---

## Overview

**Wog-WinMacChanger** is a lightweight Windows tool that changes the MAC address of an external USB WiFi adapter by modifying its **Network Adapter GUID** in the Windows registry. It was developed and used to demonstrate a **MAC spoofing bypass attack** against a Raspberry Pi wireless access point running MAC address filtering — successfully bypassing the filter and sharing internet through the spoofed adapter.

> ⚠️ **This tool is intended for authorized security testing only.** Only use it on networks and devices you own or have explicit written permission to test.

---

## Key Features

- Spoof the MAC address of an **external USB WiFi adapter** on Windows
- Uses GUID-based registry modification — no third-party driver required
- Demonstrated successfully in a real-world pentest scenario against a Raspberry Pi AP
- Simple, portable `.exe` — no installation needed
- Useful for network security research, MAC filter bypass testing, and privacy-conscious network switching

---

## Compatibility

| Component | Requirement |
|---|---|
| OS | Windows 10 / 11 |
| Adapter | **External USB WiFi adapter** |
| Built-in WiFi | ❌ Not supported on most laptops |

> **Why external adapters only?**  
> Built-in WiFi chipsets on most laptops enforce the hardware MAC at the firmware/driver level, which prevents registry-level spoofing. External USB adapters typically expose the MAC address through a writable registry GUID entry, making them compatible with this approach.

---

## Real-World Use Case

This tool was used as part of a self-directed pentest on a **Raspberry Pi WiFi access point** configured with MAC address filtering:

1. The Pi's AP was set to only allow specific MAC addresses.
2. Wog-WinMacChanger was used to spoof the MAC address of an external USB adapter to match an allowlisted address.
3. The spoofed adapter successfully bypassed the MAC filter, joined the network, and shared the internet connection — demonstrating that **MAC filtering alone is not a reliable security control**.

**Takeaway:** MAC address filtering should be treated as a minor inconvenience, not a security boundary. Always layer it with stronger controls (WPA3, certificate-based auth, network segmentation, etc.).

---

## Usage

1. Download `Spoof.exe` from the repository.
2. Connect your **external USB WiFi adapter**.
3. Run `Spoof.exe` as **Administrator**.
4. Follow the on-screen prompts to select the adapter and enter the desired MAC address.
5. Reconnect the adapter or restart the network service to apply the change.

> **Tip:** To revert to the original MAC, simply delete the modified registry value and reconnect the adapter — Windows will reload the hardware default.

---

## How It Works

Windows stores network adapter configuration, including the locally administered MAC address (`NetworkAddress`), in the registry under:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-...}\<adapter GUID>
```

Wog-WinMacChanger locates the correct GUID entry for your external adapter and writes a new `NetworkAddress` value. On reconnect, Windows applies this value as the adapter's MAC instead of the hardware-burned address.

---

## Limitations

- Does **not** work on most built-in (integrated) WiFi adapters
- Requires **Administrator privileges**
- Some adapters may ignore the registry override depending on their driver implementation
- Changes are **not persistent** across full driver reinstalls (only across reconnects)

---

## Ethical & Legal Notice

This tool is published for **educational and authorized security research purposes only**.

- ✅ Use on your own hardware and networks
- ✅ Use with explicit written permission from the network owner
- ❌ Do not use to bypass access controls on networks you do not own
- ❌ Do not use for any illegal activity

Unauthorized MAC spoofing may violate the Computer Fraud and Abuse Act (CFAA), the Computer Misuse Act, or equivalent laws in your jurisdiction.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Author

**cajx-it** — built as part of a personal WiFi security research project on a Raspberry Pi testbed.
