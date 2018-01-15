1. https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/


一、删除intelbacklight.kext驱动，dsdt和ssdt中的PNLF补丁（名字好像是intelbacklight）需要删除clover中的addpnlf补丁也要删除。
二、利用RehabMan给的Patches.xcodeproj，生成ssdt-pnlf.aml
生成到build目录下。
在config.plist中的SortedOrder中加入ssdt-pnlf.aml，将ssdt-pnlf.aml放入efi——clover——apci——patch中
三、在config.plist——KextsToPatch中加入AppleBacklightInjector补丁rehabman最近更新了补丁
<dict>
<key>Comment</key>
<string>change F%uT%04x to F%uTxxxx for AppleBacklightInjector.kext (credit RehabMan)</string>
<key>Disabled</key>
<false/>
<key>Find</key>
<data>
RiV1VCUwNHgA
</data>
<key>Name</key>
<string>com.apple.driver.AppleBacklight</string>
<key>Replace</key>
<data>
RiV1VHh4eHgA
</data>
</dict>
四、将AppleBacklightInjector.kext安装到S/L/E或L/E中，重建缓存，efi的kext中无效（10.13系统放在clover的kext中即可，不需要安到系统里）


注意事项：
1、ssdt-pnlf.aml中已将GFX0重命名为IGPU，在ddst和ssdt中没有打GFX0改为IGPU补丁，这个ssdt会失效2、ssdt-pnlf.aml一定要放在有IGPU调用的dsdt和ssdt后，有依赖关系，否则失效
3、有生成的变频ssdt，将变频ssdt请放到sortedorder最后，ssdt—pnlf放到变频ssdt之前，否则变频会失效
4、建议在完全驱动核显后，再调试亮度，楼主升级10.13后核显失效，亮度也失效，但在驱动核显后，亮度恢复（部分朋友出现显卡已经驱动，显存正常，但是亮度失效，播放视频花屏，这个只需要重建一下缓存即可）