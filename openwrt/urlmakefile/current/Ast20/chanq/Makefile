include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel
PKG_VERSION:=master
PKG_RELEASE:=1

PKG_SOURCE:=master.zip
PKG_SOURCE_URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel/archive/refs/heads/
PKG_CAT:=unzip
PKG_HASH:=skip

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/$(PKG_NAME)
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Asterisk Mobile Telephony Module
  DEPENDS:=+asterisk20 $(ICONV_DEPENDS) +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +alsa-lib
endef

define Package/$(PKG_NAME)/description
 Asterisk channel driver for mobile telephony.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)/usr/lib/asterisk/modules
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)/usr/lib/asterisk/modules/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))