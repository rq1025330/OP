# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2020 Lean <coolsnowwolf@gmail.com>
# Copyright (C) 2021-2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=autocore
PKG_FLAGS:=nonshared
PKG_RELEASE:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_TARGET_bcm27xx \
	CONFIG_TARGET_bcm53xx \
	CONFIG_TARGET_mvebu \
	CONFIG_TARGET_x86

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/target.mk

define Package/autocore
  TITLE:=auto core loadbalance script.
  DEPENDS:=@(aarch64||arm||i386||i686||x86_64) \
    +TARGET_bcm27xx:bcm27xx-userland \
    +TARGET_bcm53xx:nvram \
    +(TARGET_mediatek_filogic||TARGET_mvebu):mhz \
    +TARGET_x86:ethtool \
    +TARGET_x86:lm-sensors
endef

define Build/Compile
endef

define Package/autocore/install
	$(INSTALL_DIR) $(1)/etc
	$(CP) ./files/10_system.js $(1)/etc/rpcd_10_system.js
	$(CP) ./files/20_memory.js $(1)/etc/rpcd_20_memory.js
	$(CP) ./files/30_network.js $(1)/etc/rpcd_30_network.js
	$(CP) ./files/luci $(1)/etc/rpcd_luci
	$(CP) ./files/luci-base.json $(1)/etc/rpcd_luci-base.json

	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/090-cover-index_files $(1)/etc/uci-defaults/

	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/admin_status/index/
	$(INSTALL_DATA) ./files/nvme_status.htm $(1)/usr/lib/lua/luci/view/admin_status/index/
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/admin_status/index
	$(INSTALL_DATA) ./files/disc_status.htm $(1)/usr/lib/lua/luci/view/admin_status/index/

ifneq ($(filter i386 i686 x86_64, $(ARCH)),)
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/autocore $(1)/etc/init.d/
endif

	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) ./files/cpuinfo $(1)/sbin/
	$(INSTALL_BIN) ./files/getcpu $(1)/sbin/
ifneq ($(filter ipq% mediatek% qualcommax%, $(TARGETID)),)
	$(INSTALL_BIN) ./files/tempinfo $(1)/sbin/
endif

	$(INSTALL_DIR) $(1)/usr/share/rpcd/acl.d
	$(CP) ./files/luci-mod-status-autocore.json $(1)/usr/share/rpcd/acl.d/

ifeq ($(filter ipq806x%, $(TARGETID)),)
	$(INSTALL_DIR) $(1)/www/luci-static/resources/view/status/include
endif
endef

$(eval $(call BuildPackage,autocore))
