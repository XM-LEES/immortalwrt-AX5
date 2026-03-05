AX5 nowifi-plus stage 1 record

Date: 2026-03-05
Branch: openclash
Base: nowifi

Goal
- Build a stable AX5 256MB nowifi image with minimal incremental features.
- Keep plugin-name suffix in artifact names for repeated test cycles.

Stage definition
- Stage 0 profile: openclash only.
  - Config record: `openclash.params.txt`
  - Suffix: `openclash-20260305-g3f55ee8a`
- Stage 1 profile: openclash + zerotier.
  - Config record: `openclash-zerotier.params.txt`
  - Suffix: `openclash-zerotier-20260305-g3f55ee8a`

Stage 1 package check (manifest)
- `luci-app-openclash`
- `luci-app-zerotier`
- `zerotier`
- Not included: `luci-app-ttyd`, `luci-theme-argon`, `luci-app-cpufreq`

Stage 1 artifact outputs (project root)
- `immortalwrt-qualcommax-ipq60xx-redmi_ax5-initramfs-uImage_openclash-zerotier-20260305-g3f55ee8a.itb` (~27M)
- `immortalwrt-qualcommax-ipq60xx-redmi_ax5-squashfs-factory_openclash-zerotier-20260305-g3f55ee8a.ubi` (~28M)
- `immortalwrt-qualcommax-ipq60xx-redmi_ax5-squashfs-sysupgrade_openclash-zerotier-20260305-g3f55ee8a.bin` (~27M)
- `immortalwrt-qualcommax-ipq60xx-redmi_ax5_openclash-zerotier-20260305-g3f55ee8a.manifest`

Build notes
- Parallelism used: `make -j64 V=s`
- Logs:
  - `/home/lmh/Desktop/openwrt/build_openclash.log`
  - `/home/lmh/Desktop/openwrt/build_openclash-zerotier.log`

Test status
- Stage 1 boot and function test: passed.
