---
title: ANR
date: 2021-12-07 16:33:32
tags: Android
categories：Android

---

#### **ANR**



trace日志分析

ActivityManager会输出ANR信息

```java
ActivityManager: ANR in com.ecovacs.benebot
09-14 15:36:05.286   432   475 E ActivityManager: PID: 12693  // 进程pid
09-14 15:36:05.286   432   475 E ActivityManager: Reason: Broadcast of Intent { act=com.ecovacs.msgservice.humanstateinfo flg=0x10 cmp=com.ecovacs.benebot/com.ecovacs.task.broadcast.ReqBroadcastReceiver (has extras) }
09-14 15:36:05.286   432   475 E ActivityManager: Load: 4.16 / 5.15 / 4.77 // Load表明是1分钟,5分钟,15分钟CPU的负载
09-14 15:36:05.286   432   475 E ActivityManager: CPU usage from 145344ms to 0ms ago (2021-09-14 15:33:37.154 to 2021-09-14 15:36:02.498):
09-14 15:36:05.286   432   475 E ActivityManager:   91% 230/rild: 13% user + 78% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   29% 199/logd: 16% user + 12% kernel / faults: 59 minor
09-14 15:36:05.286   432   475 E ActivityManager:   24% 223/surfaceflinger: 14% user + 10% kernel / faults: 1473 minor
09-14 15:36:05.286   432   475 E ActivityManager:   12% 236/audioserver: 10% user + 1.6% kernel / faults: 441 minor
09-14 15:36:05.286   432   475 E ActivityManager:   9.6% 252/logcat: 5.1% user + 4.4% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   5.9% 432/system_server: 3.8% user + 2% kernel / faults: 6592 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0.1% 243/media.codec: 0% user + 0% kernel / faults: 3826 minor
09-14 15:36:05.286   432   475 E ActivityManager:   2.1% 740/sdcard: 0.1% user + 2% kernel / faults: 3 minor
09-14 15:36:05.286   432   475 E ActivityManager:   1.4% 34/ksoftirqd/5: 0% user + 1.4% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 233/mediaserver: 0% user + 0% kernel / faults: 3185 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0.9% 29/ksoftirqd/4: 0% user + 0.9% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.9% 247/logcat: 0.4% user + 0.5% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.7% 195/dhd_dpc: 0% user + 0.7% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.6% 6996/kworker/u13:1: 0% user + 0.6% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.4% 1524/com.ecovacs.admin: 0.2% user + 0.2% kernel / faults: 4015 minor 2 major
09-14 15:36:05.286   432   475 E ActivityManager:   0.4% 7/rcu_preempt: 0% user + 0.4% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.3% 164/mmcqd/1: 0% user + 0.3% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.3% 246/netd: 0% user + 0.2% kernel / faults: 444 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0.3% 50/cfinteractive: 0% user + 0.3% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.3% 10534/kworker/u13:2: 0% user + 0.3% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.2% 196/dhd_rxf: 0% user + 0.2% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.2% 684/com.android.systemui: 0.1% user + 0% kernel / faults: 330 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0.1% 11684/kworker/u13:0: 0% user + 0.1% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.1% 10146/kworker/u12:1: 0% user + 0.1% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0.1% 194/dhd_watchdog_th: 0% user + 0.1% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 673/com.android.phone: 0% user + 0% kernel / faults: 17 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0% 5622/kworker/u12:3: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 9880/kworker/5:1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 12471/kworker/u12:2: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 948/wpa_supplicant: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 10/migration/0: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 13/migration/1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 222/servicemanager: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 235/zygote: 0% user + 0% kernel / faults: 952 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0% 3/ksoftirqd/0: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 11448/kworker/0:1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 8/rcu_sched: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 109/irq/41-ff650000: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 221/lmkd: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 262/kworker/5:1H: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 18/migration/2: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 148/irq/38-rockchip: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 725/com.google.android.inputmethod.pinyin: 0% user + 0% kernel / faults: 22 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0% 10040/kworker/0:0: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 19/ksoftirqd/2: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 24/ksoftirqd/3: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 40/kconsole: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 159/irq/47-rga: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 251/autofix: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 1//init: 0% user + 0% kernel / faults: 1 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0% 14/ksoftirqd/1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 23/migration/3: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 249/batteryd: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 315/kworker/1:1H: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 1234/com.ecovacs.lelink: 0% user + 0% kernel / faults: 10 minor
09-14 15:36:05.286   432   475 E ActivityManager:   0% 12115/kworker/3:1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:   0% 12267/kworker/1:0: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12646/kworker/5:0: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12650/kbase_event: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12668/kworker/2:1: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12693/com.ecovacs.benebot: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12748/kbase_event: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12816/com.ecovacs.msgservice: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12829/com.ecovacs.faceservice: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12849/com.ecovacs.setting: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12892/com.ecovacs.tts: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12969/kbase_event: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 12999/com.ecovacs.onlineasrservice: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 13158/com.ecovacs.benebot:filedownloader: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 13217/kbase_event: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager:  +0% 13344/kworker/u13:3: 0% user + 0% kernel
09-14 15:36:05.286   432   475 E ActivityManager: 59% TOTAL: 35% user + 23% kernel + 0% iowait + 0.5% softirq
09-14 15:36:05.286   432   475 E ActivityManager: CPU usage from 1940ms to 2458ms later (2021-09-14 15:36:04.439 to 2021-09-14 15:36:04.957):
09-14 15:36:05.286   432   475 E ActivityManager:   96% 230/rild: 13% user + 83% kernel
09-14 15:36:05.286   432   475 E ActivityManager:     98% 265/rild: 15% user 
```







**系统日志：**

```java
09-15 08:57:46.139   452   634 I WindowManager: Input event dispatching timed out sending to com.ecovacs.benebot/com.ecovacs.task.activity.NaviPointListActivity.  Reason: Waiting to send non-key event because the touched window has not finished processing certain input events that were delivered to it over 500.0ms ago.  Wait queue length: 48.  Wait queue head age: 5505.2ms.
    ...
09-15 08:57:49.835   452   492 E ActivityManager: ANR in com.ecovacs.benebot (com.ecovacs.benebot/com.ecovacs.task.activity.NaviPointListActivity)
09-15 08:57:49.835   452   492 E ActivityManager: PID: 1167
09-15 08:57:49.835   452   492 E ActivityManager: Reason: Input dispatching timed out (Waiting to send non-key event because the touched window has not finished processing certain input events that were delivered to it over 500.0ms ago.  Wait queue length: 48.  Wait queue head age: 5505.2ms.)
09-15 08:57:49.835   452   492 E ActivityManager: Load: 14.31 / 15.16 / 9.6
09-15 08:57:49.835   452   492 E ActivityManager: CPU usage from 78778ms to 0ms ago (2021-09-15 08:56:27.448 to 2021-09-15 08:57:46.226):
09-15 08:57:49.835   452   492 E ActivityManager:   88% 1762/com.ecovacs.onlineasrservice: 75% user + 13% kernel / faults: 10596 minor
09-15 08:57:49.835   452   492 E ActivityManager:   84% 233/rild: 11% user + 72% kernel / faults: 1 minor
09-15 08:57:49.835   452   492 E ActivityManager:   68% 1167/com.ecovacs.benebot: 57% user + 10% kernel / faults: 122537 minor 115 major
09-15 08:57:49.835   452   492 E ActivityManager:   50% 1488/com.ecovacs.msgservice: 38% user + 11% kernel / faults: 237766 minor 2 major
09-15 08:57:49.835   452   492 E ActivityManager:   26% 227/surfaceflinger: 13% user + 12% kernel / faults: 1639 minor
09-15 08:57:49.835   452   492 E ActivityManager:   18% 240/cameraserver: 6.1% user + 12% kernel / faults: 369326 minor
09-15 08:57:49.835   452   492 E ActivityManager:   16% 239/audioserver: 13% user + 2.3% kernel / faults: 43 minor
09-15 08:57:49.835   452   492 E ActivityManager:   15% 203/logd: 5.9% user + 9.3% kernel / faults: 227 minor
09-15 08:57:49.835   452   492 E ActivityManager:   14% 745/sdcard: 0.5% user + 14% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   10% 452/system_server: 7.1% user + 3% kernel / faults: 2710 minor 31 major
09-15 08:57:49.835   452   492 E ActivityManager:   6.2% 255/logcat: 3.1% user + 3% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   6% 245/media.codec: 3.1% user + 2.8% kernel / faults: 3334 minor 1 major
09-15 08:57:49.835   452   492 E ActivityManager:   5.1% 167/mmcqd/1: 0% user + 5.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   1.8% 199/dhd_rxf: 0% user + 1.8% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   1.7% 198/dhd_dpc: 0% user + 1.7% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   1.3% 1658/com.ecovacs.tts: 0.8% user + 0.4% kernel / faults: 2717 minor 17 major
09-15 08:57:49.835   452   492 E ActivityManager:   1.2% 29/ksoftirqd/4: 0% user + 1.2% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   1% 66/kswapd0: 0% user + 1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.7% 176/kworker/u13:1: 0% user + 0.7% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.6% 5040/kworker/u12:2: 0% user + 0.6% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.6% 4201/kworker/u12:4: 0% user + 0.6% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.6% 3/ksoftirqd/0: 0% user + 0.6% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.5% 51/kworker/u12:1: 0% user + 0.5% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.5% 248/netd: 0.1% user + 0.4% kernel / faults: 424 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.5% 34/ksoftirqd/5: 0% user + 0.5% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.5% 5046/kworker/u13:0: 0% user + 0.5% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.4% 7/rcu_preempt: 0% user + 0.4% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.4% 249/logcat: 0.1% user + 0.2% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.4% 4141/kworker/u13:2: 0% user + 0.4% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.3% 1473/com.ecovacs.admin: 0.2% user + 0.1% kernel / faults: 16 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.3% 109/irq/41-ff650000: 0% user + 0.3% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.3% 50/cfinteractive: 0% user + 0.3% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.3% 3584/kworker/1:0: 0% user + 0.3% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 1693/com.zhaobenshu.rob: 0.2% user + 0% kernel / faults: 386 minor 1 major
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 197/dhd_watchdog_th: 0% user + 0.2% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 4689/kworker/1:2: 0% user + 0.2% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 225/lmkd: 0% user + 0.1% kernel / faults: 3 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 108/irq/42-ff650000: 0% user + 0.2% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.2% 1529/com.troncell.apppod: 0.1% user + 0% kernel / faults: 310 minor 6 major
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 8/rcu_sched: 0% user + 0.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 160/irq/47-rga: 0% user + 0.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 679/com.android.phone: 0% user + 0% kernel / faults: 26 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 691/com.android.systemui: 0% user + 0% kernel / faults: 53 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 10/migration/0: 0% user + 0.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 24/ksoftirqd/3: 0% user + 0.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 236/mediaserver: 0% user + 0% kernel / faults: 25 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 548/kworker/u12:3: 0% user + 0.1% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0.1% 1100/com.ecovacs.setting: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 200/kworker/0:1H: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 3698/kworker/0:3: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 985/wpa_supplicant: 0% user + 0% kernel / faults: 1 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0% 14/ksoftirqd/1: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 150/irq/38-rockchip: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 1502/com.ecovacs.faceservice: 0% user + 0% kernel / faults: 16 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0% 201/kworker/2:1H: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 226/servicemanager: 0% user + 0% kernel / faults: 2 minor
09-15 08:57:49.835   452   492 E ActivityManager:   0% 395/kworker/1:1H: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 13/migration/1: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 19/ksoftirqd/2: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 111/irq/44-ff660000: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 184/kworker/4:1H: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 732/com.google.android.inputmethod.pinyin: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 1413/android.process.media: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 18/migration/2: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 23/migration/3: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 47/kworker/4:1: 0% user + 0% kernel
09-15 08:57:49.835   452   492 E ActivityManager:   0% 223/
```

**trace.txt日志：**

```java
----- pid 1167 at 2021-09-15 08:57:46 -----
...
"main" prio=5 tid=1 Native
  | group="main" sCount=1 dsCount=0 obj=0x73a18e58 self=0xe7208f00
  | sysTid=1167 nice=-10 cgrp=default sched=0/0 handle=0xe9f8e534
  | state=S schedstat=( 77319345623 48041722934 177099 ) utm=6187 stm=1543 core=4 HZ=100
  | stack=0xff723000-0xff725000 stackSize=8MB
  | held mutexes=
  kernel: (couldn't read /proc/self/task/1167/stack)
  native: #00 pc 00017404  /system/lib/libc.so (syscall+28)
  native: #01 pc 000b6e49  /system/lib/libart.so (_ZN3art17ConditionVariable16WaitHoldingLocksEPNS_6ThreadE+92)
  native: #02 pc 003f3abb  /system/lib/libart.so (_ZN3artL12GoToRunnableEPNS_6ThreadE+230)
  native: #03 pc 003f39ad  /system/lib/libart.so (_ZN3art12JniMethodEndEjPNS_6ThreadE+8)
  native: #04 pc 0001c529  /system/framework/arm/boot.oat (Java_java_io_FileDescriptor_sync__+84)
  at java.io.FileDescriptor.sync(Native method)
  at com.ecovacs.common.LogWriter.writeString(SourceFile:10)
  - locked <0x038dff6a> (a com.ecovacs.common.LogWriter)
  at com.ecovacs.common.Trace.TRACE(SourceFile:16)
  - locked <0x0d443c5b> (a java.lang.Class<com.ecovacs.common.Trace>)
  at com.ecovacs.common.Trace.i(SourceFile:1)
  - locked <0x0d443c5b> (a java.lang.Class<com.ecovacs.common.Trace>)
  at com.ecovacs.task.presenter.TimeoutPresenter.stopTimeout(SourceFile:1)
  at com.ecovacs.task.activity.NaviPointListActivity$initData$2$2.onScrollStateChanged(SourceFile:2)
  at androidx.recyclerview.widget.RecyclerView.dispatchOnScrollStateChanged(SourceFile:8)
  at androidx.recyclerview.widget.RecyclerView.setScrollState(SourceFile:4)
  at androidx.recyclerview.widget.RecyclerView.onInterceptTouchEvent(SourceFile:39)
  at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2175)
  at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:2632)
  at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2264)
  at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:2632)
  at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2264)
  at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:2632)
  at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2264)
  at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:2632)
  at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2264)
  at com.android.internal.policy.DecorView.superDispatchTouchEvent(DecorView.java:414)
  at com.android.internal.policy.PhoneWindow.superDispatchTouchEvent(PhoneWindow.java:1808)
  at android.app.Activity.dispatchTouchEvent(Activity.java:3091)
  at com.ecovacs.task.activity.TaskActivity.dispatchTouchEvent(SourceFile:5)
  at com.android.internal.policy.DecorView.dispatchTouchEvent(DecorView.java:376)
  at android.view.View.dispatchPointerEvent(View.java:10243)
  at android.view.ViewRootImpl$ViewPostImeInputStage.processPointerEvent(ViewRootImpl.java:4438)
  at android.view.ViewRootImpl$ViewPostImeInputStage.onProcess(ViewRootImpl.java:4306)
  at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:3853)
  at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:3906)
  at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:3872)
  at android.view.ViewRootImpl$AsyncInputStage.forward(ViewRootImpl.java:3999)
  at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:3880)
  at android.view.ViewRootImpl$AsyncInputStage.apply(ViewRootImpl.java:4056)
  at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:3853)
  at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:3906)
  at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:3872)
  at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:3880)
  at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:3853)
  at android.view.ViewRootImpl.deliverInputEvent(ViewRootImpl.java:6247)
  at android.view.ViewRootImpl.doProcessInputEvents(ViewRootImpl.java:6221)
  at android.view.ViewRootImpl.enqueueInputEvent(ViewRootImpl.java:6182)
  at android.view.ViewRootImpl$WindowInputEventReceiver.onInputEvent(ViewRootImpl.java:6350)
  at android.view.InputEventReceiver.dispatchInputEvent(InputEventReceiver.java:185)
  at android.view.InputEventReceiver.nativeConsumeBatchedInputEvents(Native method)
  at android.view.InputEventReceiver.consumeBatchedInputEvents(InputEventReceiver.java:176)
  at android.view.ViewRootImpl.doConsumeBatchedInput(ViewRootImpl.java:6321)
  at android.view.ViewRootImpl$ConsumeBatchedInputRunnable.run(ViewRootImpl.java:6373)
  at android.view.Choreographer$CallbackRecord.run(Choreographer.java:874)
  at android.view.Choreographer.doCallbacks(Choreographer.java:686)
  at android.view.Choreographer.doFrame(Choreographer.java:615)
  at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:860)
  at android.os.Handler.handleCallback(Handler.java:755)
  at android.os.Handler.dispatchMessage(Handler.java:95)
  at android.os.Looper.loop(Looper.java:154)
  at android.app.ActivityThread.main(ActivityThread.java:6121)
  at java.lang.reflect.Method.invoke!(Native method)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:912)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:802)

```





























1. SharedPreferences不管是apply还是commit，存储大量数据都有可能造成ANR（apply异步，commit同步阻塞）
2. [File.isFile()、File.exists()导致的ANR](https://www.jianshu.com/p/f39ef0531797)









https://www.jianshu.com/p/bacd320b36b3

https://juejin.cn/post/6844903715313303565
