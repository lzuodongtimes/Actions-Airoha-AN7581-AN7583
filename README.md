Actions-Airoha-AN7581-AN7583
XG-040G-MD (AN7581)
XG-040G-MF (AN7583)

**对作者源码做了什么**

删掉 `airoha/patches-6.18/980-revert-fraglist-gro-by-default.patch`（恢复 fraglist-GRO 默认开，解决掉速）；给 md 补三行 ARTIFACTS，让它能出 preloader/bl31；换 Aurora 主题、删 LuCI「插件」页和「交换机 VLAN」菜单、删 fkwnopd、包名 `ponwrt-` 改 `immortalwrt-`；只勾 an7581 和 an7583 两个目标的全部设备。
