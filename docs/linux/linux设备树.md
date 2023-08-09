

## 主要作用
1. 使Linux内核和硬件能够解耦。（在linux内核2.6之前，linux中设备的信息的一些驱动要编译进内核，这导致不同的开发板的linux内核都要重新编译，以适应开发板中的硬件。引入设备树之后，内核可以单独编译，硬件设备用设备树（dts文件）来描述，并用dtc工具单独编译，并编译出dtb文件。这样在系统启动的时候，再去加载dtb文件，从而实现内核和硬件的解耦）
2. 
https://zhuanlan.zhihu.com/p/476561682

https://www.bilibili.com/video/BV1t3411M7kn/?spm_id_from=333.337.search-card.all.click&vd_source=d31a858cc26ae1ffa19e14058b339f40