#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

# Требуемое имя пакета:
PKG_NAME:=asterisk20-chan-quectel

# Стабильная, читаемая версия для имени .ipk:
PKG_VERSION:=2025.03.24~3d45c7f0
PKG_RELEASE:=1

# Пин по exact commit (full SHA).
# Commit patch (primary source): https://github.com/IchthysMaranatha/asterisk-chan-quectel/commit/3d45c7f072131296a7e3c1a4faf5bb18751dbd87.patch
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=3d45c7f072131296a7e3c1a4faf5bb18751dbd87
PKG_SOURCE_DATE:=2025-03-24

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=YourName <you@example.com>

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/$(PKG_NAME)
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Asterisk 20 channel driver for Quectel EC25 (UAC)
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel

  # ВАЖНО: asterisk нужен и как runtime, и как build-order (headers/staging).
  # Список зависимостей по вашему требованию + sqlite3:
  DEPENDS:=+asterisk $(ICONV_DEPENDS) +libsqlite3 \
    +alsa-lib \
    +kmod-usb-audio +kmod-usb-acm +kmod-usb-serial-option \
    +libusb-1.0 +usb-modeswitch
endef

define Package/$(PKG_NAME)/description
Asterisk channel driver for Quectel modems (EC25/EC2x/EG9x).
Designed for voice; with Quectel UAC it can use USB Audio Class (USB sound card).
endef

CONFIGURE_ARGS+= \
  --with-asterisk=$(STAGING_DIR)/usr/include \
  --with-astversion=20

ifeq ($(CONFIG_BUILD_NLS),y)
  CONFIGURE_ARGS+=--with-iconv=$(ICONV_PREFIX)/include
else
  CONFIGURE_ARGS+=--with-iconv=$(TOOLCHAIN_DIR)/include
endif

MAKE_FLAGS+=LD="$(TARGET_CC)"

CONFIGURE_VARS += \
  DESTDIR="$(MODULES_DIR)" \
  ac_cv_type_size_t=yes \
  ac_cv_type_ssize_t=yes

# КЛЮЧЕВОЙ ФИКС:
define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Package/$(PKG_NAME)/conffiles
/etc/asterisk/quectel.conf
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk/quectel.conf

	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)$(MODULES_DIR)
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
  echo
  echo "Note: ensure 'asterisk' user has access to /dev/ttyUSB* and /dev/snd/*."
  echo "You may need to add user 'asterisk' to groups like dialout/audio (device-dependent)."
  echo
fi
exit 0
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
