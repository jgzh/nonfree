include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=ufsd-mipseb
PKG_VERSION:=8.9.0
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

ifeq ($(ARCH),mips)
	HAL_TARGET:=mips-be-elf
endif

define KernelPackage/fs-ufsd
	SUBMENU:=Filesystems
	TITLE:=Paragon NTFS&HFS+ filesystem support
	DEFAULT:=n
	DEPENDS:=+kmod-nls-base +kmod-nls-utf8 @TARGET_ar71xx
	FILES:= \
		$(PKG_BUILD_DIR)/ufsd_wrapper.ko \
		$(PKG_BUILD_DIR)/ufsd.ko
	AUTOLOAD:=$(call AutoLoad,50,ufsd_wrapper ufsd)
endef

define KernelPackage/fs-ufsd/description
  Paragon NTFS&HFS+ filesystem support
endef

MAKE_OPTS:= -C $(LINUX_DIR) \
		ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		TARGET="$(HAL_TARGET)" \
		M="$(PKG_BUILD_DIR)" \
		CONFIG_UFSD_FS=m \
		modules

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) -r src/. $(PKG_BUILD_DIR)
endef

define Build/Compile
	$(MAKE) $(MAKE_OPTS)
endef

$(eval $(call KernelPackage,fs-ufsd))

