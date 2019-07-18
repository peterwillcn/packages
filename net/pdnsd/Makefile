#
# Copyright (C) 2019 Xiaobo Tian
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=pdnsd
PKG_VERSION:=1.2.9a
PKG_RELEASE=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-par.tar.gz
PKG_SOURCE_URL:=http://members.home.nl/p.a.rombouts/$(PKG_NAME)/releases/
PKG_HASH:=bb5835d0caa8c4b31679d6fd6a1a090b71bdf70950db3b1d0cea9cf9cb7e2a7b

PKG_MAINTAINER:=Xiaobo Tian <peterwillcn@gmail.com>
PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk

define Package/pdnsd
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  DEPENDS:=+libpthread
  TITLE:=Proxy DNS Server
  URL:=http://members.home.nl/p.a.rombouts/pdnsd/
endef

define Package/pdnsd/description
	pdnsd can be used with applications that do dns lookups,
	is an IPv6 capable proxy DNS server with permanent caching
	(the cache contents are written to hard disk on exit) that
	is designed to cope with unreachable or down DNS servers
	(for example in dial-in networking).
endef

CONFIGURE_ARGS += \
	$(if $(CONFIG_IPV6),--enable-ipv6,--disable-ipv6) \
	--with-cachedir=/tmp/pdnsd

TARGET_CFLAGS += \
	-I$(STAGING_DIR)/usr/include

define Package/pdnsd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/pdnsd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/pdnsd-ctl/pdnsd-ctl $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) ./files/pdnsd.conf $(1)/etc/pdnsd.conf
#	$(INSTALL_CONF) $(PKG_BUILD_DIR)/doc/pdnsd.conf $(1)/etc/pdnsd.conf.example
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/pdnsd.init $(1)/etc/init.d/pdnsd
endef

$(eval $(call BuildPackage,pdnsd))
