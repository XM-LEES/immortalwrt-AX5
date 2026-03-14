# AX5 Branch Profile Diffconfig

This directory stores branch-specific `diffconfig` files for Redmi AX5 builds.

- `nowifi-base.diffconfig`: no Wi-Fi packages, IPQ/NSS set to 256/LOW.
- `nowifi-plus.diffconfig`: base + `luci-app-openclash` + `luci-app-zerotier` + `zerotier`.
- `nowifi-plus-oc.diffconfig`: plus profile used on OC branch.

Apply example:

```bash
cp config/ax5-profiles/<profile>.diffconfig .config
make defconfig
```
