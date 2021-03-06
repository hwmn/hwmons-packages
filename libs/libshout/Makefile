#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libshout
PKG_VERSION:=2.2.2
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://downloads.us.xiph.org/releases/libshout/
PKG_MD5SUM:=4f75fc9901c724b712c371c9a1e782d3

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS += $(FPIC) -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Package/libshout
  SECTION:=libs
  DEPENDS:= +libspeex +libtheora +libvorbis +libvorbisidec +libpthread
  CATEGORY:=Libraries
  TITLE:=Library which can be used to write a source client like ices
  URL:=http://www.icecast.org/download.php
endef

define Package/libshout/description
	libshout allows applications to easily communicate and broadcast
	to an Icecast streaming media server. It handles the socket connections,
	metadata communication, and data streaming for the calling application,
	and lets developers focus on feature sets instead of implementation
	details.
endef

define Build/Configure
	$(call Build/Configure/Default, \
		VORBIS_CFLAGS="-I$(STAGING_DIR)/usr/include/tremor/" \
		VORBIS_LIBS="$(TARGET_LDFLAGS) -lvorbis -lvorbisidec" \
		--enable-shared \
		--enable-static \
	)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/shout
	$(CP) $(PKG_INSTALL_DIR)/usr/include/shout/shout.h $(1)/usr/include/shout
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libshout.{a,so*} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(SED) 's|/usr/include|$(STAGING_DIR)/usr/include|' \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/shout.pc
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/shout.pc $(1)/usr/lib/pkgconfig/		
endef

define Package/libshout/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libshout.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libshout))
