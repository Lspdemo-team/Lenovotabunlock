

# 联想自助解锁bootloader

### 不是华为，如果你是华为的话，看看热闹就行

前言:不要问类似**usb打不开咋办**，**连不上电脑咋办**，看完解决你大部分问题

你要准备的东西

**脑子**

**usb数据线**

adb工具(包括fastboot)

一个较大的C盘 建议预留出（平板空间大小+10G）

网络

你的平板

#### ·第一步：准备好各种包

[链接](https://mirrors.lolinet.com/firmware/lenovo)

鉴于twrp啥的你可以去xda或者谷鸽去搜对应的

#### ·第二步：下载qfil

[这个qfil比较稳定 笔者也在用的](https://github.com/YoungToday/Lenovotabunlock/blob/main/QFil.zip)

---

解压都会吧 自己吧刷机包都解压了嗷

#### 第三步：平板进入深刷模式

电脑先打开qfil，你可以看见**No Port Available**

1.将数据线插在电脑上，但**不连接平板**

2.完成第一步之后，平板按住音量上键，**插入数据线**

什么时候松开？

当qfil上面**No Port Available**显示9008字样或者choose a existing port时候松开就行

FAQs

```
Q:choose a existing port？
A:SelectPort找到平板对应com接口选上就行

Q:自己开机了咋办
A:重新试试

Q:还不好使咋办
A:adb reboot edl
A:目前对于<=安卓9的管控(MiaMdm)还不至于封9008 但是当安卓版本>=10时候(csdk,Commercial-SDK)时候有一定概率会通过内置ndk去操控lenovoraw分区达到封锁9008的效果(至少我们不是)
```

----

**Qualcomm HS-USB QDLoader (9008)**

#### 第四步：平板进入firehouse模式备份分区

**Select Build Type**

选flat build 

**Select Programmer**-browse

定位到你自己解压好的刷机包里，找到prog_emmc_firehose_****_ddr.mbn

选好之后 在上面Tools进入Partition Manager

FAQs

```
Q:Sahara *****failed
A:win7开机按f8 找到禁用驱动强制签名啥的回车在开机
A:win8-11重启电脑按住shift键 选择高级启动(具体百度) 电脑开机按f7(禁用驱动强制签名)在开机
```

备份的分区在%appdata%/qualcomm/qfil/可见

以下操作在完成后 打开目录 记得**改个名**

找到**system**选择read image

找到**boot**选择read image

找到**recovery**选择read image

找到**vendor**选择read image

找到**vbmeta/vbmetabak**选择read image

userdata备份也没啥用 都加密了（除非你要无伤回管控（我

存上，非常重要 你的管控系统就是它

如果你是刷机大佬，一天刷好几百个gsi的，建议您全部备份一下，万一哪个分区丢了就不好玩了（尤其串号

---

#### 第五步：刷机

备份好了之后退出firehouse

平板会重启 重复第三步回来9008，找到flat build 相信你已经很熟练了

loadxml 找你刷机包位置 就是rawprogram_update.xml

patch0.xml点上就行

这时候 平板找个稳定位置 不要碰数据线啥的 电脑也不要乱动

**点Download**开始刷机

出现异常的状况：**Status出现黑底白字了**

FAQ

```
Q:中途掉了 刷机失败了咋办
A:重复第三步直接到第五步就行
```

刷机完了 关闭qfil 然后平板拔线 长按电源到震动 静等开机

五分钟还在第一屏Lenovo界面就重刷吧肯定凉了

---

#### 第六步:解锁bootloader

开机后 在设置里打开开发者(不会就百度，五次版本号)

adb记得打开

**警告：可能会用到科学上网**

oem解锁开关成功打开就行，如果是灰色的 就要科学上网

```
adb reboot bootloader
fastboot oem unlock-go
```

会恢复出厂,bl已经解锁了

---

#### 第七步:装twrp，magisk

fastboot模式下

注：twrp.img是你下载的twrp，一定下对应版本的(x605fc刷x605f就不行)

```
fastboot flash recovery twrp.img
```

[magisk下载](https://github.com/topjohnwu/Magisk)

咋装都能装上 想想办法哈哈哈


这玩意都会吧 自己研究研究就搞上了 我就不说了

### 回管控

adb reboot edl 直接到9008

找到你管控系统的system boot vbmeta vbmetabak vendor刷上

然后到fastboot或者twrp清一下data分区就行，或者你要是备份管控的userdata分区刷上也行



end
