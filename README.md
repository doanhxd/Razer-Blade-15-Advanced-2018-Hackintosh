# Razer Blade 15 Advanced 2018 Hackintosh - OpenCore
**Note: I WILL NOT RESPONSIBLE IF YOU MESS UP YOUR COMPUTER USING THIS GUIDE!**

SUPPORT
---
**I have no access anymore to Razer Blade notebooks and not be able to test properly and update documentation. I open for any cooperation and will try maintain this repository as much as possible. Please fill free to create Pull Requests.**

Intro
---

![About this Mac](https://github.com/doanhmaple/Razer-Blade-15-Advanced-2018-Hackintosh/raw/master/images/About_Mac.png)

This BIOS is actual only for Razer Blade 15 Advanced 2018

| | Version |
| ---: | :--- |
| ``OpenCore`` | 0.6.0 (Beta) |
| ``Catalina`` | 10.15.6 (19G73) |
| ``System BIOS`` | 1.08 |
| ``EC FW`` | 1.02 |
| ``MCU FW`` | 1.00.00.00 |

## Disclaimer
This repository has no other purpose but sharing.
I want to share my hackintosh configuration for this particular laptop with the whole world.
This is not a step by step guide, rather a thing that can greatly help you on your way of turning this or similar laptop into a hackintosh.
Though you can just grab and use this if you have same or very similar laptop model, I greatly encourage you not do so, but instead read all the things I will mention below and get some knowledge.

Hardware
---

**Razer Blade 15 Advanced 2018 - RZ09-0238**

| | Specifications | macOS 10.15 Catalina compatibility |
| ---: | :--- | :--- |
| ``Chipset`` | Mobile Intel HM370 | No issues |
| ``CPU`` | Intel Core i7-8750H processor, 6 Cores / 12 Threads, 2.2GHz / 4.1GHz, 9MB Cache | No issues |
| ``Memory`` | 16GB dual-channel DDR4-2667MHz, up to 64GB | No issues |
| ``GPU`` | Intel UHD Graphics 630 | No issues |
| ``dGPU`` | Nvidia 1060 Max-Q (6GB GDDR5 VRAM) | Nvidia Drivers absent for Catalina. ACPI should be patched to disable dGPU |
| ``Storage`` | Samsung SM961 256GB NVMe M.2 | No issues  |
| ``Screen`` | 15.6" Full HD 60Hz, 1920 x 1080 IPS |  No issues |
| ``Webcam`` | Windows Hello built-in IR HD webcam (1MP / 720P) |  No issues. Windows Hello is not supported in macOS |
| ``WiFi`` | Intel Wireless-AC 9560NGW | Drivers absent for macOS. Should replaced |
| ``Input & Output`` | USB 3.1 Gen 1 (USB-A) x3 | No issues |
| | Thunderbolt 3 (USB-C) | No issues |
| | HDMI 2.0B | HDMI connected directly to Nvidia GPU and will not work in macOS |
| | Mini DisplayPort 1.4 | Mini DisplayPort connected directly to Nvidia GPU and will not work in macOS |
| ``Soundboard`` | Realtek ALC298 | No issues. ACPI patch should be added to solve sleep issue |
| ``Battery`` | 80Wh | About 3-5h after proper Power Management configuration.  ACPI should be patched to enable battery stats |
| ``Keyboard`` | Per-key RGB powered by Razer Chroma N-Key rollover backlit | No issues. Original Razer Chroma software absent for macOS. Many thanks to [BlvckBytes](https://github.com/BlvckBytes) for [MenuBar app](https://github.com/BlvckBytes/RazerControl/releases) to control Razer Blade keyboard and logo RGB lighting |
| ``Touchpad`` | Precision Glass | No issues. ACPI should be patched to enable trackpad |
| ``Dimensions`` | 17.8mm x 235mm x 355mm | |
| ``Weight`` | 2.21 kg | ACPI patches will not help with this. |
| ``Power`` | 230W power adapter | |

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

**Note: I do recommend to use at least 1Tb NVMe for dual boot with Windows 10.**

**RAM**

| Memory module | Modules size | Speed | CL | Amazon URL | Confirmation |
| ---: | :--- | :--- | :--- | :--- | :--- |
| ``Ballistix Sport LT 32GB`` | 2x16Gb | 2666 | CL16 | [Amazon](https://www.amazon.com/gp/product/B06XRBS4Y5/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1) | [stonevil](https://www.tonymacx86.com/members/stonevil.254235/) |
| ``Kingston Technology HyperX Impact 32GB`` | 2x16Gb | 2666 | CL15 | [Amazon](https://www.amazon.com/dp/B01NAL3TYY/?coliid=I3Q9P4ZU9V435H&colid=1ZGSQH2G88154&psc=1&ref_=lv_ov_lig_dp_it) | [Razer Blade 15 Advanced RAM upgrade](https://www.reddit.com/r/razer/comments/c1c9wl/razer_blade_15_advanced_ram_upgrade/) |

## Gratitude

* [Dortania](https://dortania.github.io/vanilla-laptop-guide/) - for vanilla guides
* [Acidanthera](https://github.com/acidanthera) - for OpenCore and lots of kexts
* [RehabMan](https://github.com/RehabMan) - for ACPI patching guides
* [Stonevil](https://github.com/stonevil) - for BIOS mod and hardware suggestions

### Where to begin

**Use stonevil's guide for modding BIOS**
* [Razer Blade Advanced early 2019 macOS 10.14/10.15 Hackintosh](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh)

**Use Dortania's guide for doing config.plist and create Bootable USB**

* [OpenCore Vanilla laptop guide](https://dortania.github.io/vanilla-laptop-guide)
and you must have some research then...

### Specific things

* **BIOS** You will have to change DVMT pre-alloc size to 64mb, and you can't do that via stock BIOS, please see how-to in here - https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh#bios-unlock
* **GPIO pinning** There are hotpatches & ssdts that might be specific for a particular laptop, I think trackpad GPIO pinning might be one of them, please check your pin number as per - https://voodooi2c.github.io/#GPIO%20Pinning/GPIO%20Pinning, and modify SSDT-I2C if needed (currently pin number is set to 0x64 in there)

---


**Note: KHÔNG CHỊU TRÁCH NHIỆM NẾU LAPTOP BẠN GẶP VẤN ĐỀ KHI SỬ DỤNG GUIDE NÀY!**

HỖ TRỢ
---

**Để cài đặt thành công và hoàn thiện Hackintosh `clean` nhất, bạn nên tìm hiểu thêm từ nhiều nguồn tài liệu khác nhau và đặt câu hỏi với `Google`**

**Note: Phiên bản BIOS này chỉ phù hợp với Laptop Razer Blade 15 Advanced 2018**

| | Phiên bản |
| ---: | :--- |
| ``OpenCore`` | 0.6.0 (Beta) |
| ``Catalina`` | 10.15.6 (19G73) |
| ``System BIOS`` | 1.08 |
| ``EC FW`` | 1.02 |
| ``MCU FW`` | 1.00.00.00 |

## Lưu ý
Repo này không có mục đích gì khác ngoài chia sẻ.
Tôi chỉ muốn chia sẻ kinh nghiệm và trải nghiệm của mình về việc cài đặt Hackintosh với mọi người.
Đây không phải bài hướng dẫn chi tiết (step-by-step) mà các bạn nên tìm hiểu thêm nếu muốn cài đặt thành công và hoàn chỉnh Hackintosh.
Nếu bạn có cùng mã Laptop với tôi thì hãy copy - paste EFI vào USB cài đặt của bạn và cài thôi.

Phần cứng
---

**Razer Blade 15 Advanced 2018 - RZ09-0238**

| | Thông số | Tương thích với macOS 10.15 - Catalina |
| ---: | :--- | :--- |
| ``Chipset`` | Mobile Intel HM370 | Không lỗi |
| ``CPU`` | Intel Core i7-8750H processor, 6 Cores / 12 Threads, 2.2GHz / 4.1GHz, 9MB Cache | Không lỗi |
| ``RAM`` | 16GB dual-channel DDR4-2667MHz, có thể nâng cấp lên 64GB | Không lỗi |
| ``GPU`` | Intel UHD Graphics 630 | Không lỗi |
| ``dGPU`` | Nvidia 1060 with Max-Q Design (6GB GDDR5 VRAM) | Nvidia Drivers không hỗ trợ Catalina. Cần phải patch DSDT/SSDT để tắt dGPU |
| ``Ổ cứng`` | Samsung SM961 256GB NVMe M.2 | Không lỗi  |
| ``Màn hình`` | 15.6" Full HD 60Hz (1920 x 1080) IPS |  Không lỗi |
| ``Webcam`` | Windows Hello built-in IR HD Webcam (1MP / 720P) |  Không lỗi. Windows Hello không hỗ trợ trên macOS |
| ``WiFi`` | Intel Wireless-AC 9560NGW | Drivers chưa hỗ trợ macOS. Nên thay card wifi hoặc dùng USB wifi có hỗ trợ macOS |
| ``Input & Output`` | USB 3.1 Gen 1 (USB-A) x3 | Không lỗi |
| | Thunderbolt 3 (USB-C) | Không lỗi. Nên tạo USB Map để trách lỗi vặt như sleep/wake |
| | HDMI 2.0B | HDMI xuất từ card Nvidia nên sẽ không hoạt động trên macOS |
| | Mini DisplayPort 1.4 | Mini DisplayPort xuất từ card Nvidia nên sẽ không hoạt động trên macOS |
| ``Âm thanh`` | Realtek ALC298 | Không lỗi. Cần patch DSDT/SSDT để sử lỗi sleep/wake |
| ``Pin`` | 80Wh | Khoảng 3-5h sử dụng tác vụ thông thường.  Cần patch DSDT/SSDT để hiển thị thông số Pin |
| ``Bàm phím`` | Per-key RGB powered by Razer Chroma N-Key rollover backlit | No issues. Original Razer Chroma software absent for macOS. Many thanks to [BlvckBytes](https://github.com/BlvckBytes), [MenuBar app](https://github.com/BlvckBytes/RazerControl/releases) to control Razer Blade keyboard and logo RGB lighting |
| ``Touchpad`` | Precision Glass | Không lỗi. Phải patch DSDT/SSDT để kích hoạt Trackpad |
| ``Kích thước`` | 17.8mm x 235mm x 355mm | Không lỗi |
| ``Cân nặng`` | 2.21 kg | ACPI patch không có tác dụng với cái này :D |
| ``Sạc`` | 230W Power Adapter | Không lỗi |

Nâng cấp phần cứng và các công cụ đi kèm
---

Card ``WiFi`` tích hợp không hoạt động trên macOS. Hãy tham khảo một số phương án thay thế sau, mình lấy thông tin từ cộng đồng đã trải nghiệm và sử dụng.


**Phụ kiện**

| Phụ kiện | Mô tả | Liên kết Amazon |
| ---: | :--- | :--- |
| ``Chuột`` | Trackpad sẽ không hoạt động trong quá trình cài đặt macOS | [Amazon](https://www.amazon.com/AmazonBasics-3-Button-Wired-Mouse-Black/dp/B005EJH6RW/ref=sr_1_3?keywords=amazon+basic+mouse&qid=1561714362&s=gateway&sr=8-3) |
| ``USB`` | USB để tạo bộ cài Hackintosh (8-16GB) | [Amazon](https://www.amazon.com/gp/product/B076GXJJRD/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1) |
| ``cáp chuyển USB-A sang USB-C`` | Để Mapping cổng USB | [Amazon](https://www.amazon.com/AmazonBasics-Type-C-Gen1-Female-Adapter/dp/B01GGKYXVE/ref=pd_hpb_a2a_sims_6/130-2479265-2893400?_encoding=UTF8&pd_rd_i=B01GGKYYT0&pd_rd_r=54b9f737-919c-11e9-b9d7-6915ce2a8dc3&pd_rd_w=j9bw6&pd_rd_wg=IVvh1&pf_rd_p=bfc589eb-d865-496f-a10b-5e00902c2113&pf_rd_r=G68JVK6HBAFKEDA75MYY&refRID=G68JVK6HBAFKEDA75MYY&th=1) |


**WiFi**

| Card WiFi | Mô tả | eBay hoặc AliExpress | Chứng nhận |
| ---: | :--- | :--- | :--- |
| ``BCM94352Z (DW-1560)`` | Đề xuất. Có 2 ăng-ten. Không lỗi. Cần thêm kext phù hợp. Giá cả phải chăng \$24-60 | [eBay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=BCM94352Z+DW-1560&_sacat=0&rt=nc&LH_BIN=1) | [cộng đồng](https://osxlatitude.com/forums/topic/11138-inventory-of-supportedunsupported-wireless-cards-2-sierra-catalina/) |
| ``BCM943602BAED (DW-1830)`` | Có 3 ăng-ten. RBA15 chỉ có 2 ăng-ten nên không khuyến khích lắm. Giá khoảng \$60-120 trên AliExpress | [AliExpress](https://www.aliexpress.com/wholesale?catId=0&initiative_id=SB_20190707194727&SearchText=BCM943602BAED+DW1830&switch_new_app=y) | [cộng đồng](https://osxlatitude.com/forums/topic/11138-inventory-of-supportedunsupported-wireless-cards-2-sierra-catalina/) |

**Ổ cứng**

| NVMe | Hỗ trợ 4K | Amazon | Chứng nhận |
| ---: | :--- | :--- | :--- |
| ``Samsung EVO 970 NVMe`` | KHÔNG | [Amazon](https://www.amazon.com/gp/product/B07DB942BT/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1) | [cộng đồng](https://www.tonymacx86.com) |
| ``Samsung EVO 970 Pro NVMe`` | KHÔNG | [Amazon](https://www.amazon.com/Samsung-PCI-Express-Solid-State-V-NAND/dp/B07DFJ3YQR/ref=sr_1_4?keywords=Samsung+970+EVO+Pro&qid=1560233808&s=electronics&sr=1-4) | [cộng đồng](https://www.tonymacx86.com) |
| ``Samsung EVO 970 Plus NVMe`` | KHÔNG | [Amazon](https://www.amazon.com/Samsung-970-EVO-Plus-MZ-V7S1T0B/dp/B07MFZY2F2/ref=sr_1_3?keywords=Samsung+EVO+970+Plus+NVMe&qid=1561343834&s=gateway&sr=8-3) | [Do the Samsung 970 Evo Plus drives work? New Firmware Available for testing 5/20/19](https://www.tonymacx86.com/threads/do-the-samsung-970-evo-plus-drives-work-new-firmware-available-for-testing-5-20-19.270757/page-13#post-1959914) |
| ``Sabrent Rocket NVMe`` | CÓ | [Amazon](https://www.amazon.com/gp/product/B07LGF54XR/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) | [stonevil](https://www.tonymacx86.com/members/stonevil.254235/) |
| ``WD Black SN750 NVMe`` | - | [Amazon](https://www.amazon.com/BLACK-SN750-500GB-Internal-Gaming/dp/B07MQ468S8/ref=sxin_3_ac_d_rm?keywords=wd%2Bblack%2Bnvme&pd_rd_i=B07MH2P5ZD&pd_rd_r=0be71a8a-a79d-4ce3-ad47-102a5ee16a25&pd_rd_w=9ZCWD&pd_rd_wg=dlFxu&pf_rd_p=0bc35c17-1e0d-4808-b361-20ab11b00973&pf_rd_r=0CKT4MYE9A5QZ8AJYEVK&qid=1560233421&s=gateway&th=1) | [cộng đồng](https://www.tonymacx86.com) |
| ``HP EX900 M.2 NVMe`` | - | [Amazon](https://www.amazon.com/HP-EX900-Internal-Solid-5Xm46Aa/dp/B07MFBNMF1/ref=sr_1_3?keywords=HP+EX900+NVME+1TB+drive&qid=1561283379&s=gateway&sr=8-3) | [konohasaint](https://www.tonymacx86.com/members/konohasaint.88998/) |
| ``Samsung PM981`` | CÓ | Có sẵn trên Razer Blade | [suyukai](https://www.tonymacx86.com/members/suyukai.2249983/) |

macOS hỗ trợ native 4k blocks. Kiểm tra **NVMe format**.
Performance tested with [Blackmagic Disk Speed Test](https://apps.apple.com/us/app/blackmagic-disk-speed-test/id425264550?mt=12). Samsung EVO 970 1Tb NVMe and Sabrent Rocket 1Tb NMVe have the same Read/Write performance. But Samsung EVO stays about 8-12° C hotter on heave load. Even with additional passive cooling.

**Note: Nên sử dụng ít nhất 512GB SSD để dualboot với Windows 10.**


**RAM**

| RAM | Dung lượng | Tốc độ | CL | Amazon | Chứng nhận |
| ---: | :--- | :--- | :--- | :--- | :--- |
| ``Ballistix Sport LT 32GB`` | 2x16GB | 2667 | CL16 | [Amazon](https://www.amazon.com/gp/product/B06XRBS4Y5/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1) | [stonevil](https://www.tonymacx86.com/members/stonevil.254235/) |
| ``Kingston Technology HyperX Impact 32GB`` | 2x16GB | 2667 | CL15 | [Amazon](https://www.amazon.com/dp/B01NAL3TYY/?coliid=I3Q9P4ZU9V435H&colid=1ZGSQH2G88154&psc=1&ref_=lv_ov_lig_dp_it) | [Razer Blade 15 Advanced RAM upgrade](https://www.reddit.com/r/razer/comments/c1c9wl/razer_blade_15_advanced_ram_upgrade/) |

## Tài liệu tham khảo

Many thanks to

* [Dortania](https://dortania.github.io/vanilla-laptop-guide/) - for vanilla guides
* [Acidanthera](https://github.com/acidanthera) - for OpenCore and lots of kexts
* [RehabMan](https://github.com/RehabMan) - for ACPI patching guides
* [Stonevil](https://github.com/stonevil) - for BIOS mod and hardware suggestions

### Bắt đầu từ đâu?

**Làm theo hướng dẫn cách Mod BIOS của stonevil(Razer Blade only)**
* [Razer Blade Advanced early 2019 macOS 10.14/10.15 Hackintosh](https://github.com/stonevil/Razer_Blade_Advanced_early_2019_Hackintosh)

**Làm theo hướng dẫn của Dortania để tạo file config.plist và tạo USB Boot**
* [OpenCore Vanilla laptop guide](https://dortania.github.io/vanilla-laptop-guide)

Các bạn nên tìm hiểu thêm từ nhiều nguồn để hoàn thiện Hackintosh hơn nhé... :D

### Thông tin thêm
---

[VoodooI2C](https://voodooi2c.github.io/#GPIO%20Pinning/GPIO%20Pinning)
``GPI0 pinning`` Bao gồm hot patch & SSDT cho việc patch nóng Trackpad. Hiện tại patch SSDT-I2C trong EFI của mình đang là pinning `0x64`

### Todo
1. Prepare for macOS Big Sur
2. Update EFI - OpenCore
3. Almost d
