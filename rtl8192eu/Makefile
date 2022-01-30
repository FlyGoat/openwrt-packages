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

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

USER_EXTRA_CFLAGS = -DBACKPORT

ifneq ($(LINUX_KARCH), x86)
USER_EXTRA_CFLAGS += -DCONFIG_MINIMAL_MEMORY_USAGE 
endif

USER_EXTRA_CFLAGS += -DCONFIG_RTW_IOCTL_SET_COUNTRY
USER_EXTRA_CFLAGS += -DCONFIG_IOCTL_CFG80211
USER_EXTRA_CFLAGS += -DRTW_USE_CFG80211_STA_EVENT
USER_EXTRA_CFLAGS += -DCONFIG_MP_INCLUDED
USER_EXTRA_CFLAGS += -DCONFIG_WEXT_PRIV
USER_EXTRA_CFLAGS += -DCONFIG_WIRELESS_EXT
USER_EXTRA_CFLAGS += -DCFG80211_VERSION_CODE=0x50f08

MAKE_FEATURES:= \
	CONFIG_POWER_SAVING="n"

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

MAKE_OPTS:= \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(KERNEL_CROSS)" \
	KSRC="$(LINUX_DIR)" \
	KVER="$(LINUX_VERSION)" \
	M="$(PKG_BUILD_DIR)" \
	MODULE_NAME="8192eu" \
	USER_EXTRA_CFLAGS="$(USER_EXTRA_CFLAGS)" \
	NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" 
	KBUILD_EXTRA_SYMBOLS="${STAGING_DIR}/usr/include/mac80211/Module.symvers" \
	$(MAKE_FEATURES)


define KernelPackage/$(PKG_NAME)
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek RTL8192EU support
  DEPENDS:=@USB_SUPPORT +kmod-lib80211 +kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/8192eu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8192eu)
endef

define KernelPackage/$(PKG_NAME)/description
 Kernel modules for the Realtek RTL-8192EU USB 802.11bgn
 wireless USB adapters
endef


define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_OPTS) modules
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
