include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-subconverter
PKG_VERSION:=1.5.1
PKG_RELEASE:=1

LUCI_TITLE:=Subconverter LuCI Interface
LUCI_DEPENDS:=+luci-base +luci-compat
LUCI_PKGARCH:=all

include ../../luci.mk

# 安装后脚本
define Package/luci-app-subconverter/postinst
#!/bin/sh
if [ -z "${IPKG_INSTROOT}" ]; then
	/etc/init.d/subconverter enable >/dev/null 2>&1
fi
exit 0
endef

# 卸载前脚本
define Package/luci-app-subconverter/prerm
#!/bin/sh
if [ -z "${IPKG_INSTROOT}" ]; then
	/etc/init.d/subconverter stop >/dev/null 2>&1
	/etc/init.d/subconverter disable >/dev/null 2>&1
fi
exit 0
endef

# call BuildPackage - OpenWrt buildroot signature
$(eval $(call BuildPackage,$(PKG_NAME)))