# This is free software, licensed under the Apache License, Version 2.0 .
#

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-adguardhome
PKG_VERSION:=1.8
PKG_RELEASE:=13

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/luci-app-adguardhome
	SECTION:=luci
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=LuCI app for adguardhome
	PKG_MAINTAINER:=<https://github.com/rufengsuixing/luci-app-adguardhome>
	PKGARCH:=all
	DEPENDS:=+ca-certs +curl +wget-ssl +PACKAGE_$(PKG_NAME)_INCLUDE_binary:adguardhome
endef

LUCI_DESCRIPTION:=LuCI support for AdGuardHome


define Package/$(PKG_NAME)/config
config PACKAGE_$(PKG_NAME)_INCLUDE_binary
	bool "Include Binary File"
	default y
endef

PKG_CONFIG_DEPENDS:= CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_binary

define Package/luci-app-adguardhome/conffiles
/usr/share/AdGuardHome/links.txt
/etc/config/AdGuardHome
/etc/config/AdGuardHome.yaml
endef


define Package/luci-app-adguardhome/postinst
#!/bin/sh
	chmod 755 /usr/share/AdGuardHome/*
	/etc/init.d/AdGuardHome enable >/dev/null 2>&1
	enable=$(uci get AdGuardHome.AdGuardHome.enabled 2>/dev/null)
	if [ "$enable" == "1" ]; then
		/etc/init.d/AdGuardHome reload
	fi
	rm -f /tmp/luci-indexcache
	rm -f /tmp/luci-modulecache/*
exit 0
endef

define Package/luci-app-adguardhome/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
     /etc/init.d/AdGuardHome disable
     /etc/init.d/AdGuardHome stop
uci -q batch <<-EOF >/dev/null 2>&1
	delete ucitrack.@AdGuardHome[-1]
	commit ucitrack
EOF
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
