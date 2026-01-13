include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-subconverter
PKG_VERSION:=1.5
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=luci
	CATEGORY:=LuCI
	PKGARCH:=all
	SUBMENU:=3. Applications
	TITLE:=Subconverter
	DEPENDS:=+luci-base +luci +luci-compat
endef

define Package/$(PKG_NAME)/description
	a control panel for subweb and subconverter
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/subconverter
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/subconverter

	$(INSTALL_DATA) ./luasrc/controller/subconverter.lua $(1)/usr/lib/lua/luci/controller/ 
	$(INSTALL_DATA) ./luasrc/view/subconverter/*.htm $(1)/usr/lib/lua/luci/view/subconverter/ 
	
	# 安装配置文件和启动脚本 
	$(CP) ./root/etc/subconverter/* $(1)/etc/subconverter/ 
	$(INSTALL_BIN) ./root/etc/init.d/subconverter $(1)/etc/init.d/ 
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
# 仅在真机安装时运行，不在固件编译期间运行
if [ -z "$${IPKG_INSTROOT}" ]; then
	chmod 755 /etc/subconverter/subconverter 
	/etc/init.d/subconverter enable
fi
exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
# 清理计划任务并停止进程 
if [ -z "$${IPKG_INSTROOT}" ]; then
	/etc/init.d/subconverter stop
	/etc/init.d/subconverter disable
	(crontab -l 2>/dev/null | grep -v '/etc/init.d/subconverter') | crontab - 
	killall -q subconverter 2>/dev/null 
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk
$(eval $(call BuildPackage,$(PKG_NAME)))
