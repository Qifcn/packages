#
# Copyright (C) 2009-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libsodium
PKG_BASE_VERSION:=1.0.18
PKG_RELEASE:=3

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/jedisct1/libsodium.git
PKG_SOURCE_DATE:=2022-02-27
PKG_SOURCE_VERSION:=2e9b4c47c8cf6759a11a90c2468036e47674b29a
PKG_MIRROR_HASH:=7bec6dcc5f5591db34b103efff6989feea573b63913c6ce400db1c583c475554

PKG_VERSION:=$(PKG_BASE_VERSION)-$(PKG_SOURCE_DATE)-$(call version_abbrev,$(PKG_SOURCE_VERSION))

PKG_MAINTAINER:=Damiano Renfer <x9w2n7xnu@relay.firefox.com>
PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_FLAGS:=no-mips16
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libsodium
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=P(ortable|ackageable) NaCl-based crypto library
  URL:=https://github.com/jedisct1/libsodium
endef

define Package/libsodium/description
  NaCl (pronounced "salt") is a new easy-to-use high-speed software library for network communication, encryption, decryption, signatures, etc.
  NaCl's goal is to provide all of the core operations needed to build higher-level cryptographic tools.
  Sodium is a portable, cross-compilable, installable, packageable fork of NaCl (based on the latest released upstream version nacl-20110221), with a compatible API.
  The design choices, particularly in regard to the Curve25519 Diffie-Hellman function, emphasize security (whereas NIST curves emphasize "performance" at the cost of security), and "magic constants" in NaCl/Sodium have clear rationales.
  The same cannot be said of NIST curves, where the specific origins of certain constants are not described by the standards.
  And despite the emphasis on higher security, primitives are faster across-the-board than most implementations of the NIST standards.
endef

define Package/libsodium/config
menu "Configuration"
	depends on PACKAGE_libsodium
	config LIBSODIUM_MINIMAL
		bool "Compile only what is required for the high-level API (no aes128ctr), should be fine in most cases."
		default y
endmenu
endef

CONFIGURE_ARGS+= \
	--disable-ssp \
	$(if $(CONFIG_LIBSODIUM_MINIMAL),--enable,--disable)-minimal

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/sodium
	$(CP) $(PKG_INSTALL_DIR)/usr/include/sodium.h $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/sodium/*.h $(1)/usr/include/sodium
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsodium.{a,so*} $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libsodium.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libsodium/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsodium.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libsodium))
