#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ola
#PKG_VERSION:=0.8.27
PKG_VERSION:=0.9.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OpenLightingProject/ola/releases/download/$(PKG_VERSION)/
PKG_SHA1SUM:=f6a81087761218063a4bb8006b73ffa407cd0170
PKG_USE_MIPS16:=0

PKG_FIXUP:=libtool
PKG_INSTALL:=1

PKG_BUILD_PARALLEL:=1

HOST_BUILD_DEPENDS:=protobuf
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/ola-$(PKG_VERSION)

#PKG_BUILD_DEPENDS:=protobuf
PKG_BUILD_DEPENDS:=+ola/host protobuf

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

#PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
#define Build/Prepare
#	rm -r $(PKG_BUILD_DIR)
#	mkdir -p $(PKG_BUILD_DIR)
#	$(CP) /home/markus/svn/open-lighting/** $(PKG_BUILD_DIR)/
#endef

#define Host/Compile
#endef

#define Host/Install
#endef

define Package/ola
  SECTION:=ola
  CATEGORY:=Open Lighting Architecture
  TITLE:=Open Lighting Architecture Daemon
  URL:=http://code.google.com/p/linux-lighting/
  DEPENDS:=+libartnet +protobuf +libmicrohttpd +libusb-1.0 +libuuid +libstdcpp +libpthread +librt +zlib +libncurses +libavahi-client +mdnsd +liblo +libusb-1.0 +libftdi
  MENU:=1
endef

define Package/ola/description
  OLA (Open Lighting Architecture) is a framework that allows applications to 
  send and receive DMX512, using various hardware devices and 'DMX over IP'
  protocols. It enables software controllers talk to DMX hardware.
endef

HOST_CONFIGURE_ARGS += \
        --disable-examples \
        --disable-unittests \

CONFIGURE_ARGS += \
	--with-ola-protoc=$(HOST_BUILD_DIR)/protoc/ola_protoc \
	--disable-dependency-tracking \
	--disable-static \
	--disable-slp \
	--disable-root-check \
	--disable-fatal-warnings \
	--disable-unittests 
#        --with-protoc=$(HOST_STAGING_DIR)/bin/protoc \
        
define Build/Configure
  $(call Build/Configure/Default,$(CONFIGURE_ARGS))
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ola $(1)/usr/include/
	mkdir -p $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/olad $(1)/usr/include/
	mkdir -p $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
endef

define Package/ola/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib/olad
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
#	$(CP) $(PKG_INSTALL_DIR)/usr/lib/olad/*.so* $(1)/usr/lib/olad
	$(INSTALL_DIR) $(1)/usr/share/olad/www
	$(CP) $(PKG_INSTALL_DIR)/usr/share/olad $(1)/usr/share
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/olad.init $(1)/etc/init.d/olad
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,ola))
