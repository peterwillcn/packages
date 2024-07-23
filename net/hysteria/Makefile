# Copyright (C) 2023 Findlay Feng <i@fengch.me>

include $(TOPDIR)/rules.mk

PKG_NAME:=hysteria
PKG_VERSION:=2.5.0
PKG_RELEASE:=1

GO_PKG:=github.com/apernet/hysteria

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://$(GO_PKG).git
PKG_MIRROR_HASH:=98c4e776dc0c14de820a7113dc39a1cbe4c2ba07b203852bd2a7a65607f93fed
PKG_SOURCE_VERSION:=app/v$(PKG_VERSION)

PKG_LICENSE:=MIT
PKG_MAINTAINER:=Findlay Feng <i@fengch.me>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-values.mk

GO_PKG_BUILD_PKG=$(GO_PKG)/app/v2
GO_PKG_LDFLAGS_X:= \
	$(GO_PKG_BUILD_PKG)/cmd.appDate=$(shell date -u '+%F.%T') \
	$(GO_PKG_BUILD_PKG)/cmd.appVersion=v$(PKG_VERSION) \
	$(GO_PKG_BUILD_PKG)/cmd.appCommit=$(PKG_SOURCE_VERSION) \
	$(GO_PKG_BUILD_PKG)/cmd.appPlatform=linux \
	$(GO_PKG_BUILD_PKG)/cmd.appType=release \
	$(GO_PKG_BUILD_PKG)/cmd.appArch=$(GO_ARCH)

include $(GO_INCLUDE_DIR)/golang-package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=A feature-packed proxy & relay tool optimized for lossy
	URL:=https://hysteria.network
endef

define Package/$(PKG_NAME)-configs
	$(call Package/$(PKG_NAME))
	TITLE:=configs
	DEPENDS:=$(PKG_NAME)
	USERID:=hysteria=102:hysteria=102
endef

define Package/$(PKG_NAME)-serverd
	$(call Package/$(PKG_NAME))
	TITLE:=server init scripts
	DEPENDS:=$(PKG_NAME) +$(PKG_NAME)-configs
endef

define Package/$(PKG_NAME)-clientd
	$(call Package/$(PKG_NAME))
	TITLE:=client init scripts
	DEPENDS:=$(PKG_NAME) +$(PKG_NAME)-configs \
		+kmod-nft-tproxy +kmod-nft-socket \
		+dnsmasq-full
endef

define Package/$(PKG_NAME)/description
	Hysteria is a feature-packed proxy & relay tool optimized for lossy,
	unstable connections (e.g. satellite networks, congested public Wi-Fi,
	connecting to foreign servers from China) powered
	by a customized protocol based on QUIC.
endef

define Package/$(PKG_NAME)-configs/description
	$(call Package/$(PKG_NAME)/description)
endef

define Package/$(PKG_NAME)-serverd/description
	$(call Package/$(PKG_NAME)/description)
endef

define Package/$(PKG_NAME)-clientd/description
	$(call Package/$(PKG_NAME)/description)
endef

define Build/Compile
	$(call GoPackage/Build/Compile)
endef

define Package/$(PKG_NAME)/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/sysctl.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/app $(1)/usr/bin/$(PKG_NAME)

	$(INSTALL_CONF) ./files/sysctl-hysteria.conf $(1)/etc/sysctl.d/11-$(PKG_NAME).conf
endef

define Package/$(PKG_NAME)-configs/install
	$(INSTALL_DIR) \
		$(1)/etc/config \
		$(1)/etc/capabilities

	$(INSTALL_CONF) ./files/hysteria.json $(1)/etc/capabilities/$(PKG_NAME).json
	$(INSTALL_CONF) ./files/hysteria.config $(1)/etc/config/$(PKG_NAME)
endef

define Package/$(PKG_NAME)-serverd/install
	$(INSTALL_DIR) \
		$(1)/etc/init.d \
		$(1)/etc/$(PKG_NAME)

	$(INSTALL_BIN) ./files/hysteria-serverd.init $(1)/etc/init.d/$(PKG_NAME)-serverd
	$(INSTALL_DATA) ./files/configs/server.yaml $(1)/etc/$(PKG_NAME)/
endef

define Package/$(PKG_NAME)-clientd/install
	$(INSTALL_DIR) \
		$(1)/etc/init.d \
		$(1)/usr/share/$(PKG_NAME)/bin \
		$(1)/etc/$(PKG_NAME)

	$(INSTALL_BIN) ./files/hysteria.init $(1)/etc/init.d/$(PKG_NAME)
	$(INSTALL_BIN) ./files/bin/hysteria-tproxy.sh $(1)/usr/share/$(PKG_NAME)/bin/set-tproxy
	$(INSTALL_DATA) ./files/configs/client.yaml $(1)/etc/$(PKG_NAME)/
endef

define Package/$(PKG_NAME)-configs/conffiles
/etc/config/$(PKG_NAME)
endef

define Package/$(PKG_NAME)-serverd/conffiles
/etc/${PKG_NAME}/
endef

define Package/$(PKG_NAME)-clientd/conffiles
/etc/${PKG_NAME}/
endef

$(eval $(call GoBinPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)-configs))
$(eval $(call BuildPackage,$(PKG_NAME)-serverd))
$(eval $(call BuildPackage,$(PKG_NAME)-clientd))
