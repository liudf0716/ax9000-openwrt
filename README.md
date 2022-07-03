# ax9000-openwrt
如何在ax9000上安装openwrt

## 如何开启ax9000的ssh

方法网上有教程，可以结合openwrt官方【参考文献中的第8条】和恩山上教程【第6条】，个人觉得openwrt官方的教程更适合开发者，需要注意的是以下几点；

1. 先根据官方的教程，配置好辅助openwrt设备的ip地址，dhcp配置及无线配置，然后重启该设备
2. 等辅助openwrt设备重启完成后，先通过浏览器登录ax9000设备，登录成功后获取其stok值:`http://192.168.31.1/cgi-bin/luci/;stok=8cef284c22c6f9853dec7159cc885792/web/home#router`，然后用该stok值替代下面的url中的stok值：

`http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/xqsystem/extendwifi_connect_inited_router?ssid=<SSID>&password=<PASSWORD>&encryption=<ENCRYPTION>enctype=<ENCTYPE>&channel=<CHANNEL>&band=<BAND>&admin_username=root&admin_password=admin&admin_nonce=xxx`

3. 将上述url值粘贴到浏览器上，访问该url地址，过一段时间后会返回下列内容：
```
{"token":"; nvram set ssh_en=1; nvram commit; echo -e 'admin\u000aadmin' | passwd root; sed -i 's/channel=.*/channel=\u0022debug\u0022/g' /etc/init.d/dropbear; /etc/init.d/dropbear start;","code":0}
```
4. ssh root@192.168.31.1，密码为admin，出现如下内容：
```
BusyBox v1.25.1 (2021-05-28 10:22:27 UTC) built-in shell (ash)

 -----------------------------------------------------
       Welcome to XiaoQiang!
 -----------------------------------------------------
  $$$$$$\  $$$$$$$\  $$$$$$$$\      $$\      $$\        $$$$$$\  $$\   $$\
 $$  __$$\ $$  __$$\ $$  _____|     $$ |     $$ |      $$  __$$\ $$ | $$  |
 $$ /  $$ |$$ |  $$ |$$ |           $$ |     $$ |      $$ /  $$ |$$ |$$  /
 $$$$$$$$ |$$$$$$$  |$$$$$\         $$ |     $$ |      $$ |  $$ |$$$$$  /
 $$  __$$ |$$  __$$< $$  __|        $$ |     $$ |      $$ |  $$ |$$  $$<
 $$ |  $$ |$$ |  $$ |$$ |           $$ |     $$ |      $$ |  $$ |$$ |\$$\
 $$ |  $$ |$$ |  $$ |$$$$$$$$\       $$$$$$$$$  |       $$$$$$  |$$ | \$$\
 \__|  \__|\__|  \__|\________|      \_________/        \______/ \__|  \__|


root@XiaoQiang:~#
root@XiaoQiang:~#
root@XiaoQiang:~# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00100000 00020000 "0:SBL1"
mtd1: 00100000 00020000 "0:MIBIB"
mtd2: 00080000 00020000 "0:BOOTCONFIG"
mtd3: 00080000 00020000 "0:BOOTCONFIG1"
mtd4: 00300000 00020000 "0:QSEE"
mtd5: 00300000 00020000 "0:QSEE_1"
mtd6: 00080000 00020000 "0:DEVCFG"
mtd7: 00080000 00020000 "0:DEVCFG_1"
mtd8: 00080000 00020000 "0:APDP"
mtd9: 00080000 00020000 "0:APDP_1"
mtd10: 00080000 00020000 "0:RPM"
mtd11: 00080000 00020000 "0:RPM_1"
mtd12: 00080000 00020000 "0:CDT"
mtd13: 00080000 00020000 "0:CDT_1"
mtd14: 00080000 00020000 "0:APPSBLENV"
mtd15: 00100000 00020000 "0:APPSBL"
mtd16: 00100000 00020000 "0:APPSBL_1"
mtd17: 00080000 00020000 "0:ART"
mtd18: 00080000 00020000 "bdata"
mtd19: 00080000 00020000 "crash"
mtd20: 00080000 00020000 "crash_syslog"
mtd21: 03800000 00020000 "rootfs"
mtd22: 03800000 00020000 "rootfs_1"
mtd23: 00100000 00020000 "cfg_bak"
mtd24: 07d80000 00020000 "overlay"
mtd25: 005ef000 0001f000 "kernel"
mtd26: 01e84000 0001f000 "ubi_rootfs"
mtd27: 00c79000 0001f000 "rootfs_data"
mtd28: 03013000 0001f000 "data"
root@XiaoQiang:~#  
```

dmesg内容
```
  
```
  
## 参考文献
1. https://opssh.cn/luyou/166.html
2. https://forum.openwrt.org/t/openwrt-support-for-xiaomi-ax9000/98908
3. https://github.com/BuzzBumbleBee/openwrt
4. https://lwn.net/ml/linux-kernel/20201222222637.3204929-1-robert.marko@sartura.hr/#t
5. https://github.com/coolsnowwolf/lede/commit/edbd8d2e9839357f3a4f0a06174d243f362b1544
6. https://supes.top/%e7%ba%a2%e7%b1%b3ax6-%e8%a7%a3%e9%94%81ssh%e5%88%b7u-boot%e4%b8%8eopenwrt%e6%95%99%e7%a8%8b/
7. https://www.right.com.cn/forum/thread-8234200-1-2.html
8. https://openwrt.org/inbox/toh/xiaomi/ax9000


