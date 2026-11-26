Actions-Airoha-AN7581-AN7583

XG-040G-MD (AN7581)

XG-040G-MF (AN7583)


对作者源码做了什么？

删掉 `airoha/patches-6.18/980-revert-fraglist-gro-by-default.patch`（恢复 fraglist-GRO 默认开，解决掉速）；给 md 补三行 ARTIFACTS，让它能出 preloader/bl31；换 Aurora 主题、删 LuCI「插件」页和「交换机 VLAN」菜单、删 fkwnopd、包名 `ponwrt-` 改 `immortalwrt-`；只勾 an7581 和 an7583 两个目标的全部设备。

删除 `fkwnopd` 并不是因为它有什么问题，问题出在**该包的 kconfig 循环依赖会让整个配置生成失败**；不改就无法继续构建。

删 `980-revert-fraglist-gro-by-default.patch`，是为了把 **fraglist GRO 的默认值从 “关” 改回 “开”**。这个动作直接对应实测过的吞吐差。
具体逻辑：
- 上游 OpenWrt 有 `target/linux/generic/hack-6.18/600-net-enable-fraglist-GRO-by-default.patch`，它的作用是让 `NETIF_F_GRO_FRAGLIST` **默认开启**。
- pbs05 又加了一个 `airoha/patches-6.18/980-revert-fraglist-gro-by-default.patch`，把默认值**翻回关闭**。两个补丁的效果相反：600 开启，980 关闭，最终由 980 决定。
- 删除 980 后，上游 600 的选择重新生效，也就是 GRO 默认开启。
关闭 GRO 会让吞吐变差的原因：GRO 会先把同一连接的小包在内核里合并成大包再处理；关闭后，桥接转发要逐包处理，每包开销上升、单核负担变重。
