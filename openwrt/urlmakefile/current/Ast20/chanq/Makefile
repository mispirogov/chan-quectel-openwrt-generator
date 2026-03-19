include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk20-chan-quectel
PKG_VERSION:=2025.03.24~3d45c7f0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=3d45c7f072131296a7e3c1a4faf5bb18751dbd87
PKG_SOURCE_DATE:=2025-03-24

PKG_FIXUP:=autoreconf
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/$(PKG_NAME)
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Asterisk 20 channel driver for Quectel EC25
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel
  DEPENDS:=+asterisk +libsqlite3 +alsa-lib +libusb-1.0 +usb-modeswitch +kmod-usb-audio +kmod-usb-acm +kmod-usb-serial-option
endef

define Package/$(PKG_NAME)/description
Asterisk channel driver for Quectel modems.
endef

CONFIGURE_ARGS += \
  --with-asterisk=$(STAGING_DIR)/usr/include \
  --with-astversion=20

ifeq ($(CONFIG_BUILD_NLS),y)
  CONFIGURE_ARGS += --with-iconv=$(ICONV_PREFIX)/include
else
  CONFIGURE_ARGS += --with-iconv=$(TOOLCHAIN_DIR)/include
endif

MAKE_FLAGS += LD="$(TARGET_CC)"

CONFIGURE_VARS += \
  DESTDIR="$(MODULES_DIR)" \
  ac_cv_type_size_t=yes \
  ac_cv_type_ssize_t=yes

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