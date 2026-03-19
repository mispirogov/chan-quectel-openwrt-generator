include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel
PKG_VERSION:=master
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_VERSION).zip
PKG_SOURCE_URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel/archive/refs/heads/
PKG_HASH:=skip

PKG_FIXUP:=autoreconf
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/$(PKG_NAME)
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel
  TITLE:=Asterisk chan_quectel (EC25 voice + UAC)

  # как минимум asterisk + iconv deps и USB/ALSA зависимости
  # Asterisk в OpenWrt 24.10.x — 20.8.1 и сам тянет libopenssl/libsqlite3 и др. citeturn11view0
  DEPENDS:=asterisk $(ICONV_DEPENDS) \
    +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option \
    +kmod-usb-audio +usb-modeswitch \
    +libusb-1.0 +alsa-lib
endef

define Package/$(PKG_NAME)/description
Asterisk channel driver for Quectel/Simcom modems. For EC25 can work with UAC USB-audio.
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

# КЛЮЧ: стандартная подготовка исходников из архива
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

$(eval $(call BuildPackage,$(PKG_NAME)))
