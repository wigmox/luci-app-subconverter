include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-subconverter
PKG_VERSION:=1.5.1
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=luci
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=Subconverter LuCI Interface
	DEPENDS:=+luci-base +luci +luci-compat
	PKGARCH:=all
endef

define Package/$(PKG_NAME)/description
	A web-based control panel for subweb and subconverter.
endef

# 准备工作：无需额外编译动作
define Build/Compile
endef

# 安装阶段：将文件放入镜像的正确位置
define Package/$(PKG_NAME)/install
	# 创建必要的目录
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/subconverter
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/subconverter

	# 1. 安装 Lua 控制器和视图文件 (使用 INSTALL_DATA 确保 644 权限)
	$(INSTALL_DATA) ./luasrc/controller/subconverter.lua $(1)/usr/lib/lua/luci/controller/
	$(CP) ./luasrc/view/subconverter/*.htm $(1)/usr/lib/lua/luci/view/subconverter/
	# 如果有语言包文件，请取消下面行的注释
	# $(CP) ./luasrc/i18n/*.lmo $(1)/usr/lib/lua/luci/i18n/ 2>/dev/null || true

	# 2. 安装二进制文件和配置 (关键修复：直接在打包阶段赋予执行权限)
	if [ -d "./root/etc/subconverter" ]; then \
		$(CP) ./root/etc/subconverter/* $(1)/etc/subconverter/; \
		chmod 755 $(1)/etc/subconverter/subconverter 2>/dev/null || true; \
	fi

	# 3. 安装启动脚本 (使用 INSTALL_BIN 确保 755 权限)
	if [ -f "./root/etc/init.d/subconverter" ]; then \
		$(INSTALL_BIN) ./root/etc/init.d/subconverter $(1)/etc/init.d/subconverter; \
	fi
endef

# 安装后脚本：规避编译环境报错
define Package/$(PKG_NAME)/postinst
#!/bin/sh
# 检查是否是在真实的路由器环境中运行 (IPKG_INSTROOT 为空代表非编译打包阶段)
if [ -z "$${IPKG_INSTROOT}" ]; then
	( . /etc/uci-defaults/luci-subconverter ) >/dev/null 2>&1
	/etc/init.d/subconverter enable >/dev/null 2>&1
fi
exit 0
endef

# 卸载前脚本：清理残留
define Package/$(PKG_NAME)/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	/etc/init.d/subconverter stop >/dev/null 2>&1
	/etc/init.d/subconverter disable >/dev/null 2>&1
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk

# 结束
$(eval $(call BuildPackage,$(PKG_NAME)))
