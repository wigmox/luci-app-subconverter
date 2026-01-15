include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-subconverter
PKG_VERSION:=1.5.1
PKG_RELEASE:=1

LUCI_TITLE:=Subconverter LuCI Interface
LUCI_DEPENDS:=+luci-base +luci-compat
LUCI_PKGARCH:=all

include $(TOPDIR)/feeds/luci/luci.mk

# 安装后脚本
define Package/$(PKG_NAME)/postinst
    #!/bin/sh
    chmod 755 /etc/subconverter/subconverter
    chmod 755 /etc/init.d/subconverter
endef

# 卸载前脚本
define Package/$(PKG_NAME)/prerm
#!/bin/sh
if [ -z "${IPKG_INSTROOT}" ]; then
    crontab -l | grep -v '/etc/init.d/subconverter' | crontab -
    killall -q /etc/subconverter/subconverter
    rm -rf /etc/subconverter
fi
exit 0
endef

# call BuildPackage - OpenWrt buildroot signature
