# Razer Blade 15 Advanced 2018 Hackintosh - OpenCore (Big Sur - 11.1)
**Note: I WILL NOT RESPONSIBLE IF YOU MESS UP YOUR COMPUTER USING THIS GUIDE!**

Intro
---

![About this Mac](https://github.com/doanhmaple/Razer-Blade-15-Advanced-2018-Hackintosh/raw/master/images/About_Mac.png)

This BIOS is actual only for Razer Blade 15 Advanced (2018)

| | Version |
| ---: | :--- |
| ``OpenCore`` | 0.6.4 (RELEASE) |
| ``Catalina`` | 10.15.7 (19H114) |
| - | - |
| ``OpenCore`` | 0.6.5 (DEBUG) |
| ``Big Sur`` | 11.1 (20C69) |
| ``System BIOS`` | 1.08 |
| ``EC FW`` | 1.02 |
| ``MCU FW`` | 1.00.00.00 |

## Disclaimer

This repository has no other purpose but sharing.
I want to share my hackintosh configuration for this particular laptop with the whole world.
This is not a step by step guide, rather a thing that can greatly help you on your way of turning this or similar laptop into a hackintosh.
Though you can just grab and use this if you have same or very similar laptop model, I greatly encourage you not do so, but instead read all the things I will mention below and get some knowledge.

## Hmmm...

* Please create [USBMap](https://github.com/corpnewt/USBMap) or `USBPort.kext` (Use Hackintool to do this) after install for best USB plug experience (uncheck SSDT-USBX-LAPTOP in config.plist or remove it when using USBMap/USBPort.kext)

* Create one-key cpufriend if you often use battery, power-plug always is not recommended for best performance 

Hardware
---

**Razer Blade 15 Advanced 2018 - RZ09-02385**

| | Specifications | macOS 10.15 Catalina & Big Sur compatibility |
| ---: | :--- | :--- |
| ``Chipset`` | Mobile Intel HM370 | No issues |
| ``CPU`` | Intel Core i7-8750H processor, 6 Cores / 12 Threads, 2.2GHz / 4.1GHz, 9MB Cache | No issues |
| ``Memory`` | 16GB dual-channel DDR4-2667MHz, up to 32GB | No issues |
| ``GPU`` | Intel UHD Graphics 630 | No issues |
| ``dGPU`` | Nvidia 1060 Max-Q (6GB GDDR5 VRAM) | Nvidia Drivers absent for Catalina. ACPI should be patched to disable dGPU |
| ``Storage`` | Samsung SM961 256GB NVMe M.2 | No issues |
| ``Screen`` | 15.6" Full HD 60Hz, 1920 x 1080 IPS |  No issues |
| ``Webcam`` | Windows Hello built-in IR HD webcam (1MP / 720P) |  No issues. Windows Hello is not supported in macOS |
| ``WiFi`` | Intel Wireless-AC 9560NGW | No issues, using [itlwm.kext](https://github.com/OpenIntelWireless/itlwm/releases) and [Heliport](https://github.com/OpenIntelWireless/HeliPort/releases). I've replaced with DW1560 (BCM94352Z) |
| ``Input & Output`` | USB 3.1 Gen 1 (USB-A) x3 | No issues |
| - | Thunderbolt 3 (USB-C) | No issues |
| - | HDMI 2.0B | HDMI connected directly to Nvidia GPU and will not work in macOS |
| - | Mini DisplayPort 1.4 | Mini DisplayPort connected directly to Nvidia GPU and will not work in macOS |
| ``Soundboard`` | Realtek ALC298 | No issues. ACPI patch should be added to solve sleep issue |
| ``Battery`` | 80Wh | About 3-5h after proper Power Management configuration.  ACPI should be patched to enable battery stats |
| ``Keyboard`` | Per-key RGB powered by Razer Chroma N-Key rollover backlit | No issues. Original Razer Chroma software absent for macOS. Many thanks to [BlvckBytes](https://github.com/BlvckBytes) for [MenuBar app](https://github.com/BlvckBytes/RazerControl/releases) to control Razer Blade keyboard and logo RGB lighting |
| ``Touchpad`` | Precision Glass | No issues. ACPI should be patched to enable trackpad |
| ``Dimensions`` | 17.8mm x 235mm x 355mm | - |
| ``Weight`` | 2.21 kg | ACPI patches will not help with this. |
| ``Power`` | 230W power adapter | - |

Hardware Upgrades and Tools
---

The bundled ``WiFI`` and ``NVMe`` is not compatible with macOS and should be replaced. Please find below the recommended replacement parts, already tested for compatibility. Usually I need to deploy for testing 4-5 node Kubernetes cluster with at least 4Gb per node. So 32GB is a necessary upgrade for me.


**Accessories**

| Accessories | Description | Amazon URL |
| ---: | :--- | :--- |
| ``USB mouse`` | Trackpad will be unavailable during macOS installation procedure | [Amazon](https://www.amazon.com/AmazonBasics-3-Button-Wired-Mouse-Black/dp/B005EJH6RW/ref=sr_1_3?keywords=amazon+basic+mouse&qid=1561714362&s=gateway&sr=8-3) |
| ``USB storage`` with at least 16GB storage | Installation USB media | [Amazon](https://www.amazon.com/gp/product/B076GXJJRD/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1) |
| ``USB-A to USB-C cable`` | For USB ports detection procedure | [Amazon](https://www.amazon.com/AmazonBasics-Type-C-Gen1-Female-Adapter/dp/B01GGKYXVE/ref=pd_hpb_a2a_sims_6/130-2479265-2893400?_encoding=UTF8&pd_rd_i=B01GGKYYT0&pd_rd_r=54b9f737-919c-11e9-b9d7-6915ce2a8dc3&pd_rd_w=j9bw6&pd_rd_wg=IVvh1&pf_rd_p=bfc589eb-d865-496f-a10b-5e00902c2113&pf_rd_r=G68JVK6HBAFKEDA75MYY&refRID=G68JVK6HBAFKEDA75MYY&th=1) |


**WiFi**

| WiFi module | Description | eBay or AliExpress URL | Confirmation |
| ---: | :--- | :--- | :--- |
| ``BCM94352Z (DW-1560)`` | Recommended. 2 antennas. No issues. Additional kext's are required. Easily to find for \$24-60 on | [eBay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=BCM94352Z+DW-1560&_sacat=0&rt=nc&LH_BIN=1) | [community](https://osxlatitude.com/forums/topic/11138-inventory-of-supportedunsupported-wireless-cards-2-sierra-catalina/) |
| ``BCM943602BAED (DW-1830)`` | 3 antennas. RBA have only 2. Works out of the box. About \$60-120 on AliExpress | [AliExpress](https://www.aliexpress.com/wholesale?catId=0&initiative_id=SB_20190707194727&SearchText=BCM943602BAED+DW1830&switch_new_app=y) | [community](https://osxlatitude.com/forums/topic/11138-inventory-of-supportedunsupported-wireless-cards-2-sierra-catalina/) |

**Storage**

| NVMe | 4k Support | Amazon URL | Confirmation |
| ---: | :--- | :--- | :--- |
| ``Samsung EVO 970 NVMe`` | NO | [Amazon](https://www.amazon.com/gp/product/B07DB942BT/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1) | [community](https://www.tonymacx86.com) |
| ``Samsung EVO 970 Pro NVMe`` | NO | [Amazon](https://www.amazon.com/Samsung-PCI-Express-Solid-State-V-NAND/dp/B07DFJ3YQR/ref=sr_1_4?keywords=Samsung+970+EVO+Pro&qid=1560233808&s=electronics&sr=1-4) | [community](https://www.tonymacx86.com) |
| ``Samsung EVO 970 Plus NVMe`` | NO | [Amazon](https://www.amazon.com/Samsung-970-EVO-Plus-MZ-V7S1T0B/dp/B07MFZY2F2/ref=sr_1_3?keywords=Samsung+EVO+970+Plus+NVMe&qid=1561343834&s=gateway&sr=8-3) | [Do the Samsung 970 Evo Plus drives work ? New Firmware Available for testing 5/20/19](https://www.tonymacx86.com/threads/do-the-samsung-970-evo-plus-drives-work-new-firmware-available-for-testing-5-20-19.270757/page-13#post-1959914) |
| ``Sabrent Rocket NVMe`` | YES | [Amazon](https://www.amazon.com/gp/product/B07LGF54XR/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) | [stonevil](https://www.tonymacx86.com/members/stonevil.254235/) |
| ``WD Black SN750 NVMe`` | - | [Amazon](https://www.amazon.com/BLACK-SN750-500GB-Internal-Gaming/dp/B07MQ468S8/ref=sxin_3_ac_d_rm?keywords=wd%2Bblack%2Bnvme&pd_rd_i=B07MH2P5ZD&pd_rd_r=0be71a8a-a79d-4ce3-ad47-102a5ee16a25&pd_rd_w=9ZCWD&pd_rd_wg=dlFxu&pf_rd_p=0bc35c17-1e0d-4808-b361-20ab11b00973&pf_rd_r=0CKT4MYE9A5QZ8AJYEVK&qid=1560233421&s=gateway&th=1) | [community](https://www.tonymacx86.com) |
| ``HP EX900 M.2 NVMe`` | - | [Amazon](https://www.amazon.com/HP-EX900-Internal-Solid-5Xm46Aa/dp/B07MFBNMF1/ref=sr_1_3?keywords=HP+EX900+NVME+1TB+drive&qid=1561283379&s=gateway&sr=8-3) | [konohasaint](https://www.tonymacx86.com/members/konohasaint.88998/) |
| ``Samsung PM981`` | NO | Bundled with Razer Blade | [suyukai](https://www.tonymacx86.com/members/suyukai.2249983/) |


macOS have native support and works better with 4k blocks. Check **NVMe format**.
Performance tested with [Blackmagic Disk Speed Test](https://apps.apple.com/us/app/blackmagic-disk-speed-test/id425264550?mt=12). Samsung EVO 970 1Tb NVMe and Sabrent Rocket 1Tb NMVe have the same Read/Write performance. But Samsung EVO stays about 8-12° C hotter on heave load. Even with additional passive cooling.

**Note: I do recommend to use at least 1TB NVMe for dual boot with Windows 10.**

**RAM**

| Memory module | Modules size | Speed | CL | Amazon URL | Confirmation |
| ---: | :--- | :--- | :--- | :--- | :--- |
| ``Ballistix Sport LT 32GB`` | 2x16Gb | 2666 | CL16 | [Amazon](https://www.amazon.com/gp/product/B06XRBS4Y5/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1) | [stonevil](https://www.tonymacx86.com/members/stonevil.254235/) |
| ``Kingston Technology HyperX Impact 32GB`` | 2x16Gb | 2666 | CL15 | [Amazon](https://www.amazon.com/dp/B01NAL3TYY/?coliid=I3Q9P4ZU9V435H&colid=1ZGSQH2G88154&psc=1&ref_=lv_ov_lig_dp_it) | [Razer Blade 15 Advanced RAM upgrade](https://www.reddit.com/r/razer/comments/c1c9wl/razer_blade_15_advanced_ram_upgrade/) |

## Gratitude

* [Dortania](https://dortania.github.io/) - for Vanilla guides
* [Acidanthera](https://github.com/acidanthera) - for OpenCore and lots of kexts
* [RehabMan](https://github.com/RehabMan) - for ACPI patching guides
* [Stonevil](https://github.com/stonevil) - for BIOS mod and hardware suggestions

### Where to begin

**Use stonevil's guide for modding BIOS**
* [Razer Blade Advanced early 2019 macOS 10.14/10.15 Hackintosh](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh#bios-unlock)

**Use Dortania's guide for doing config.plist and create Bootable USB**

* [OpenCore Vanilla laptop guide](https://dortania.github.io/OpenCore-Install-Guide)
and you must have some research then...

### Specific things

* **BIOS** You will have to change DVMT pre-alloc size to 64MB, and you can't do that via stock BIOS, please see how-to in here - [BIOS Unlock - stonevil](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh#bios-unlock)
* **GPIO pinning** There are hotpatches & ssdts that might be specific for a particular laptop, I think trackpad GPIO pinning might be one of them, please check your pin number as per - [GPI0 Pinning](https://voodooi2c.github.io/#GPIO%20Pinning/GPIO%20Pinning), and modify SSDT-I2C if needed (currently pin number is set to 0x64 in there)

---

### Todo
1. Updated macOS Big Sur (Done)...
2. Updated 10.15.7 (19H15) with OpenCore 0.6.3 RELEASE...