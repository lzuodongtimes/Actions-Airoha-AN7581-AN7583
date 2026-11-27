Actions-Airoha-AN7581-AN7583

XG-040G-MD (AN7581)

XG-040G-MF (AN7583)

常用版本，桥接模式为软桥接，路由模式为硬件卸载转发。删除了源码某些导致网速慢的错误配置。

PonWrt 全机型构建（主线）
基于 pbs05/ponwrt 源码，构建 Nokia XG-040G 系列（an7581 / an7583 两个整机配置下的全部设备）的 OpenWrt 固件。
这是长期主线版本：定时轮询上游，有更新才自动编译并发布。
这份固件做了什么
删除 airoha/patches-6.18/980-revert-fraglist-gro-by-default.patch，恢复上游 fraglist-GRO 默认开启（解决桥接转发吞吐）
为 XG-040G-MD 补三条 ARTIFACTS 声明，让它也能产出 preloader.bin / bl31-uboot.fip
LuCI 主题换成 Aurora
移除 LuCI「插件」页、「交换机 VLAN 配置」菜单
移除 fkwnopd / luci-app-fkwnopd（其 kconfig 循环依赖会让 make defconfig 直接失败）
包名前缀 ponwrt- 改为 immortalwrt-
只编 an7581 / an7583 两个整机配置
内核层面没有做流表卸载相关改动：不包含 676-net-bridge-encap-push.patch，也不启用 flow_offloading_hw。
触发方式
手动：Actions → 左侧工作流 → Run workflow
定时：每 6 小时轮询一次 pbs05/ponwrt 的 HEAD，与最近一次发布记录的 upstream_sha 比对，无变化则跳过，不产生编译
