Legacy Wireless Kexts
===========

Goal of this repo is the following:

* Bring back semi-native support without requiring system modifictaions


To do this, we'll pull the IO80211Family.kext from several versions of macOS, patch their symbols to not conflict with newer versions.

Note: when booting older OSes, there can conflict so we **highly** recommend users set the MinKernel in your config.plist(Kernel -> Add) to one of the following so the kexts are only injected in the appropriate OSes:

```
Sierra and newer   = 16.0.0
Mojave and newer   = 18.0.0
Catalina and newer = 19.0.0
Big Sur and newer  = 20.0.0
```

### Patched Files

* [Patched El Capitan Kext](./10.11.6-El-Capitan-Kexts/IO80211ElCapitan.kext.zip)
  * Recommended for legacy Broadcom users in 10.14+
  * Set MinKernel to 16.0.0
  
<details>
<summary>Supported Devices</summary>

```
Broadcom - AppleAirPortBrcm43224
   pci106b,4.   = Unreleased device
   pci14e4,4311 = BCM4311
   pci14e4,4312 = BCM4311
   pci14e4,4313 = BCM4311
   pci14e4,4318 = BCM4318
   pci14e4,4319 = BCM4318
   pci14e4,431a = Unknown
   pci14e4,4320 = BCM4306
   pci14e4,4324 = BCM4309
   pci14e4,4325 = BCM4306
   pci14e4,4328 = BCM4328
   pci14e4,432c = BCM4322
   pci14e4,432d = BCM4322
```

</details>
<br>

* [Patched High Sierra Kext](./10.13.6-High-Sierra-Kexts/IO80211HighSierra.kext.zip)
  * Recommended for Atheros users in 10.12+
  * Set MinKernel to 18.0.0
  
<details>
<summary>Supported Devices</summary>

```
Atheros - AirPortAtheros40
   pci168c,30   = AR93xx
   pci168c,2a   = AR928X
   pci106b,86   = Unreleased device
   pci168c,1c   = AR242x / AR542x
   pci168c,23   = AR5416
   pci168c,24   = AR5418
```

</details>
<br>

* [Patched Mojave Kext](./10.14.6-Mojave-Kexts/IO80211Mojave.kext.zip)
  * Recommended for BCM4322 users in 10.15+
  * Set MinKernel to 19.0.0

<details>
<summary>Supported Devices</summary>

```
Broadcom - AirPortBrcm4331
   pci14e4,432b = BCM4322
```

</details>
<br>

* [Patched Catalina Kext](./10.15.7-Catalina-Kexts/IO80211Catalina.kext.zip)
  * Recommended for BCM4331 and BCM43224 users in 11+
  * Set MinKernel to 20.0.0

<details>
<summary>Supported Devices</summary>

```
Broadcom - AirPortBrcm4360
   pci14e4,4331 = BCM4331
   pci14e4,4353 = BCM43224
```

</details>
<br>	

### Special notes

#### Unsupported Atheros Chipsets

For certain AR9285/7 and AR9280 chipsets, you will need to apply a fake Device ID to your wireless card. This is due to AirPortAtheros40 having internal PCI ID checks meaning simply expanding the device-id list won't work.

<details>
<summary>Expanding Atheros Support</summary>

To add support, grab [gfxutil](https://github.com/acidanthera/gfxutil/releases) and run the following:

```sh
/path/to/gfxutil | grep -i "pci168c:002b|pci168c:002e"
```

This should spit out something like this:

```
00:1f.6 pci168c:002e /PC00@0/PXSX@1F,6 = PciRoot(0x0)/Pci(0x1F,0x6)
```

The ending `PciRoot(0x0)/Pci(0x1F,0x6)` is what you want to add in your config.plist under `DeviceProperties -> Add` with the following properties:

| Key | Type | Value |
| :--- | :--- | :--- |
| compatible | String | "pci168c,2a" |
| device-id  | Data | 2A000000 |

</details>
<br>

#### BCM4331

Users of the 4331 chipset, note that macOS Big Sur actually still support your card partially, however will require a fake Device ID. This is a more reliable solution than using the patched IO80211 kext, however may break older versions of macOS as the fake ID is always applied in macOS(however DeviceProperties do don't exist in Windows or Linux, so no need to worry with those)

<details>
<summary>Expanding Broadcom Support</summary>

To add support, grab [gfxutil](https://github.com/acidanthera/gfxutil/releases) and run the following:

```sh
/path/to/gfxutil | grep -i "14e4:4331"
```

This should spit out something like this:

```
00:1f.6 14e4:4331 /PC00@0/PXSX@1F,6 = PciRoot(0x0)/Pci(0x1F,0x6)
```

The ending `PciRoot(0x0)/Pci(0x1F,0x6)` is what you want to add in your config.plist under `DeviceProperties -> Add` with the following properties:

| Key | Type | Value |
| :--- | :--- | :--- |
| compatible | String | "pci14e4,43ba" |
| device-id  | Data | BA430000 |

</details>
<br>
