---
title: 底层开发
weight: 2
---

{{< toc >}}

## 1. 硬件组成

对于一个基础的 LoRa 网关，主要有以下外设

- LoRa 模块: semtech sx1301 基带芯片
- 4G 模块: 移远 EC20
- PHY: KSZ8081
- 使用 SD 卡作为存储

## 2. u-boot

### 2.1 设备树开发

设备树开发可参考官方开发板设备树文件，其实也无需过多改动，因为 u-boot 的用途只是加载内核及内核设备树，所以只需确保 SD 可用就可以了

本例程中新建 imx6ull-lgw-uboot.dts, 其中 SD 相关设置如下

```
&usdhc1 {
	pinctrl-names = "default", "state_100mhz", "state_200mhz";
	pinctrl-0 = <&pinctrl_usdhc1>;
	pinctrl-1 = <&pinctrl_usdhc1_100mhz>;
	pinctrl-2 = <&pinctrl_usdhc1_200mhz>;
	keep-power-in-suspend;
	broken-cd;
	vmmc-supply = <&reg_sd1_vmmc>;
	status = "okay";
};
```

### 2.2 board 开发

通常来说，每种终端产品都要开发 board 相关的初始化代码及头文件。但是正如上述所说，u-boot 用途只是加载内核，所以可以使用官方开发板的 board 代码进行少量修改。

本例程中使用 imx6ull 芯片，所以我使用 imx6ullevk 开发板的 board 代码，并对头文件进行修改以正确加载内核及设备树

### 2.3 config 

config 同样可以参照 imx6ullevk 的默认配置，需确认启动方式，以及设备树选择

```
CONFIG_DEFAULT_DEVICE_TREE="imx6ull-gw1000-uboot"
```

同时要制作设备树文件夹下 makefile 文件补丁，添加 2.1 中新建的设备树

```
--- ./arch/arm/dts/Makefile
+++ ./arch/arm/dts/Makefile
@@ -724,7 +724,8 @@
 	imx6ull-somlabs-visionsom.dtb \
 	imx6ulz-14x14-evk.dtb \
 	imx6ulz-14x14-evk-emmc.dtb \
-	imx6ulz-14x14-evk-gpmi-weim.dtb
+	imx6ulz-14x14-evk-gpmi-weim.dtb \
+	imx6ull-gw1000-uboot.dtb
 
 dtb-$(CONFIG_ARCH_MX6) += \
 	imx6-apalis.dtb \
```

### 2.4 bbfile

需要通过 u-boot.bbappend 将上述改动添加进编译过程

```
# u-boot-imx-%
# Auto Generate by Dianshao
FILESEXTRAPATHS_prepend := "${THISDIR}/files:"
DESCRIPTION = "u-boot-imx bbappend"
LICENSE = ""
DEPENDS = "vim-native bison-native"
SRC_URI_append = "\ 
	file://mx6ullevk.h.patch \ 
	file://Makefile.patch \ 
	file://mx6ull_gw1000_defconfig \ 
	file://imx6ull-gw1000-uboot.dts \ 
"
do_configure_prepend () {
	cp ${WORKDIR}/mx6ull_gw1000_defconfig ${S}/configs
	cp ${WORKDIR}/imx6ull-gw1000-uboot.dts ${S}/arch/arm/dts
}
```

## 3. kernel

### 3.1 设备树

针对上述外设，设备树需增加以下部分内容

- SD 卡

```
&usdhc1 {
	pinctrl-names = "default", "state_100mhz", "state_200mhz";
	pinctrl-0 = <&pinctrl_usdhc1>;
	pinctrl-1 = <&pinctrl_usdhc1_100mhz>;
	pinctrl-2 = <&pinctrl_usdhc1_200mhz>;
	keep-power-in-suspend;
	broken-cd;
	vmmc-supply = <&reg_sd1_vmmc>;
	status = "okay";
};
```

- spi - lora 芯片

```
&ecspi1 {
	pinctrl-names = "default";
	pinctrl-0 = <&pinctrl_ecspi1_1 &pinctrl_ecspi1_cs_1>;
	fsl,spi-num-chipselects = <1>;
	//cs-gpios = <&gpio4 26 0>;
	status = "okay";

	spidev0: spidev@0 {
		compatible = "spidev";
		spi-max-frequency = <8000000>;
		reg = <0>;
	};	
};
```

- usb - ec20

```
&usbotg2 {
	dr_mode = "host";
	disable-over-current;
	status = "okay";
};
```

- fec - phy

```
&fec1 {
	pinctrl-names = "default";
	pinctrl-0 = <&pinctrl_enet1>;
	phy-mode = "rmii";
	phy-handle = <&ethphy0>;
	phy-reset-gpios = <&gpio5 8 GPIO_ACTIVE_LOW>;
	phy-reset-duration = <10>;
	local-mac-address = [00 06 34 01 01 01];
	status = "okay";

	mdio {
		#address-cells = <1>;
		#size-cells = <0>;

		ethphy0: ethernet-phy@2 {
			reg = <2>;
			micrel,led-mode = <1>;
			clocks = <&clks IMX6UL_CLK_ENET_REF>;
			clock-names = "rmii-ref";
		};
	};
};
```

### 3.2 config 文件

在 freescale 官方配置文件基础上，还需添加以下几点

- ppp 支持

```
CONFIG_PPP=y
CONFIG_PPP_BSDCOMP=y
CONFIG_PPP_DEFLATE=y
CONFIG_PPP_FILTER=y
CONFIG_PPP_MPPE=y
CONFIG_PPP_MULTILINK=y
CONFIG_PPP_ASYNC=y
CONFIG_PPP_SYNC_TTY=y
CONFIG_SLHC=y
```

- usb wwan 支持

```
CONFIG_USB_ACM=y
CONFIG_USB_SERIAL=y
CONFIG_USB_SERIAL_WWAN=y
CONFIG_USB_SERIAL_OPTION=y
```

- tmpfs 支持

```
CONFIG_TMPFS=y
CONFIG_TMPFS_POSIX_ACL=y
CONFIG_TMPFS_XATTR=y
CONFIG_MEMFD_CREATE=y
```

### 3.3 Makefile Patch

修改 dts 目录下 Makefile 以增加新增设备树

```
--- ./arch/arm/boot/dts/Makefile
+++ ./arch/arm/boot/dts/Makefile
@@ -692,6 +692,7 @@
 	imx6ull-phytec-segin-ff-rdk-nand.dtb \
 	imx6ull-phytec-segin-ff-rdk-emmc.dtb \
 	imx6ull-phytec-segin-lc-rdk-nand.dtb \
+	imx6ull-gw1000.dtb \
 	imx6ulz-14x14-evk.dtb \
 	imx6ulz-14x14-evk-btwifi.dtb \
 	imx6ulz-14x14-evk-gpmi-weim.dtb \
```

### 3.4 linux bbappend

增加 bbappend 将上述改动增加到编译过程

```
# linux-imx-5.10
# Auto Generate by Dianshao
FILESEXTRAPATHS_prepend := "${THISDIR}/files:"
DESCRIPTION = "linux-imx bbappend for lorawan gateway"
SRC_URI_append = "\ 
	file://Makefile.patch \
	file://gw1000.cfg \
	file://imx6ull-gw1000.dts \
"
do_configure_append () {
	cat ../*cfg >> ${B}/.config 
}

do_compile_prepend () {
	cp ${WORKDIR}/imx6ull-gw1000.dts \
	${S}/arch/${ARCH}/boot/dts
}
```

## 4. machine conf

我们在官方提供的 imx6ullevk.conf 的基础上进行修改，主要是有如下部分

- uboot 配置文件和启动方式

```
UBOOT_CONFIG = "sd"
UBOOT_CONFIG[sd] = "mx6ull_gw1000_defconfig,sdcard"
```

- uboot 编译产物名称

```
UBOOT_SUFFIX = "imx"
UBOOT_MAKE_TARGET = "u-boot.imx"
```

- fsl 内核要求宏

```
ACCEPT_FSL_EULA = "1"
```

- 内核设备树

```
KERNEL_DEVICETREE = "imx6ull-gw1000.dtb"
```

## 5. distro conf

在官方提供的 fsl-base.inc 基础上进行修改

- 名称及版本号

```
DISTRO = "dianshao"
DISTRO_NAME = "dianshao"
DISTRO_VERSION = "1.0.0"
```

- 选择 systemd 为启动方式

```
DISTRO_FEATURES_append = " systemd"
VIRTUAL-RUNTIME_init_manager = "systemd"
```