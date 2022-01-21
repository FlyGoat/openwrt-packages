include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8192eu
PKG_RELEASE=2

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/Mange/rtl8192eu-linux-driver.git
PKG_MIRROR_HASH:=skip
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-10-23
PKG_SOURCE_VERSION:=744bbe52976e51895fce2c1d4075f97a98dca2b2

PKG_MAINTAINER:=Jiaxun Yang <jiaxun.yang@flygoat.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8192eu
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8192EU devices
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/rtl8192eu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8192eu)
  PROVIDES:=kmod-rtl8192eu
endef

EXTRA_KCONFIG:= \
	CONFIG_RTL8192EU=m

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	 $(MAKE) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef


$(eval $(call KernelPackage,rtl8192eu))
