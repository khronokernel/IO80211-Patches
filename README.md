Legacy Wireless Kexts
===========

Goal of this repo is the following:

* Bring back semi-native support without requiring system modifictaions


To do this, we'll pull the IO80211Family.kext from several versions of macOS, patch their symbols to not conflict with newer versions.


### Patches Files


* [Patched High Sierra Kext]()
  
<details>
<summary>Supported Devices</summary>

* Atheros(AirPortBrcm4360):
  * pci168c,30 = AR93xx
  * pci168c,2a = AR928X
  * pci106b,86 = Unreleased device?
  * pci168c,1c = AR242x / AR542x
  * pci168c,23 = AR5416
  * pci168c,24 = AR5418
* Broadcom(AirPortBrcm4331):
  * pci14e4,4331 = BCM4331
  * pci14e4,4353 = BCM43224
  * pci14e4,432b = BCM4322
* Broadcom(AirPortBrcm4360):
  * pci14e4,4331 = BCM4331
  * pci14e4,4353 = BCM43224
* Broadcom(AirPortBrcmNIC):
  * pci14e4,43ba = BCM43602
  * pci14e4,43a3 = BCM4350
  * pci14e4,43a0 = BCM4360

</details>
<br>

* [Patched Mojave Kext]()

<details>
<summary>Supported Devices</summary>

* Broadcom(AirPortBrcm4331):
  * pci14e4,4331 = BCM4331
  * pci14e4,4353 = BCM43224
  * pci14e4,432b = BCM4322
* Broadcom(AirPortBrcm4360):
  * pci14e4,4331 = BCM4331
  * pci14e4,4353 = BCM43224
* Broadcom(AirPortBrcmNIC):
  * pci14e4,43ba = BCM43602
  * pci14e4,43a3 = BCM4350
  * pci14e4,43a0 = BCM4360

</details>
<br>

* [Patched Catalina Kext]()

<details>
<summary>Supported Devices</summary>

* Broadcom(AirPortBrcm4360):
  * pci14e4,4331 = BCM4331
  * pci14e4,4353 = BCM43224
* Broadcom(AirPortBrcmNIC):
  * pci14e4,43ba = BCM43602
  * pci14e4,43a3 = BCM4350
  * pci14e4,43a0 = BCM4360
	
</details>
<br>	

  

### Special notes

#### BCM94331

Users of the 94331 chipset, note that macOS Big Sur actually still support your card partially, however will require a fake Device ID. This is a more reliable solution than using the patched IO80211 kext, however may break older versions of macOS as the fake ID is always applied in macOS(however DeviceProperties do don't exist in Windows or Linux, so no need to worry with those)

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
