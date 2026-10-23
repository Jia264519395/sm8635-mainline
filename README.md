# sm8635-mainline

Mainline Linux kernel for Xiaomi Pad 7 Pro（muyu，SM8635 / 骁龙8s Gen3）。

> ⚠️ 本仓库是骨架。基于 `ianchb/sm8550-mainline`（sheng 分支）机械改名而来。
> 设备树 `arch/arm64/boot/dts/qcom/sm8635-xiaomi-muyu.dts` 已创建，所有设备特定节点（GPIO/I2C/中断）都标注了 TODO，需要对照 muyu 的 Android 内核 dts 逐节点替换。

## 分支

- `muyu-7.2.6`：基于 Linux 7.2.6，适配 muyu

## 构建

```sh
# 克隆
git clone -b muyu-7.2.6 https://github.com/<your-user>/sm8635-mainline.git
cd sm8635-mainline

# 配置
make ARCH=arm64 sm8635_defconfig  # TODO: 基于 sm8550.config 创建 sm8635_defconfig
# 或直接：
# cp sm8550.config .config
# scripts/kconfig/merge_config.sh .config sm8635-muyu.config

# 编译
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -j$(nproc) Image
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- sm8635-xiaomi-muyu.dtb
```

## 待完成

- [ ] 从 muyu 的 Android 内核提取 `sm8635-xiaomi-muyu.dts`，替换所有 TODO 节点
- [ ] 确认 SM8635 对应的上游 dtsi（可能是 sm8650.dtsi）
- [ ] 创建 `sm8635_defconfig`（基于 sm8550.config，去掉 sheng 特有驱动）
- [ ] 编译 boot.img：`mkbootimg --kernel Image --dtb sm8635-xiaomi-muyu.dtb -o boot_muyu_dualboot.img`
- [ ] 发布 release，命名 `7.2.6`，产物包含 `boot_muyu_dualboot.img` 和 `boot_muyu_singleboot.img`

## 参考

- [ianchb/sm8550-mainline](https://github.com/ianchb/sm8550-mainline)：sheng 内核
- [map220v/sm8550-mainline](https://github.com/map220v/sm8550-mainline)：原始 sheng 移植
- muyu Android 内核：`xiaomi/sm8635` 或 `xiaomi,muyu`
