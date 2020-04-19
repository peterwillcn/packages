include $(TOPDIR)/rules.mk

PKG_NAME:=docker-compose
PKG_VERSION:=1.25.5
PKG_RELEASE:=1

PYPI_NAME:=docker-compose
PKG_HASH:=7a2eb6d8173fdf408e505e6f7d497ac0b777388719542be9e49a0efd477a50c6

PKG_MAINTAINER:=Javier Marcet <javier@marcet.info>
PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

include ../pypi.mk
include $(INCLUDE_DIR)/package.mk
include ../python3-package.mk

PKG_USE_MIPS16:=0

PYTHON3_PKG_SETUP_ARGS:=

define Package/docker-compose
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=Docker Compose
  URL:=https://github.com/docker/compose
  DEPENDS+=+docker-ce \
      +python3 \
      +python3-cached-property \
      +python3-colorama \
      +python3-distutils \
      +python3-docopt \
      +python3-docker \
      +python3-dockerpty \
      +python3-jsonschema \
      +python3-pkg-resources \
      +python3-requests \
      +python3-setuptools \
      +python3-six \
      +python3-texttable \
      +python3-websocket-client \
      +python3-yaml
  VARIANT:=python3
endef

define Package/docker-compose/description
  Multi-container orchestration for Docker
endef

$(eval $(call Py3Package,docker-compose))
$(eval $(call BuildPackage,docker-compose))
$(eval $(call BuildPackage,docker-compose-src))
