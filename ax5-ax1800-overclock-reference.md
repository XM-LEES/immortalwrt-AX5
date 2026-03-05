## AX5/AX1800 Overclock Reference (IPQ6000/IPQ6018)

### Branch
- Working branch for OC experiments: `nowifi-plus-oc`
- Base branch kept stable: `nowifi-plus`

### What controls overclock
- `cpufreq`/LuCI can only select frequencies exposed by kernel OPP.
- Real overclock limit comes from kernel DTS OPP table and speed-bin policy.
- Current key file in this repo:
  - `target/linux/qualcommax/files/arch/arm64/boot/dts/qcom/ipq6018-common.dtsi`

### Current baseline in this repo
- Universal bins (`0xf`) up to `1512000`.
- `1608000` and `1800000` are restricted to `opp-supported-hw = <0x1>`.
- This matches a conservative upstream/bin policy.

### Mainstream community OC profiles (AX5/AX1800)
1. `safe` (recommended first test)
- Max freq: `1512000` (no DTS change)
- Risk: lowest

2. `mid` (common community target)
- Max freq: `1608000`
- DTS change:
  - set `opp-1608000000` from `<0x1>` to `<0xf>`
- Risk: medium (needs thermal/stability validation)

3. `high` (aggressive)
- Max freq: `1800000`
- DTS change:
  - set `opp-1608000000` from `<0x1>` to `<0xf>`
  - set `opp-1800000000` from `<0x1>` to `<0xf>`
- Risk: high (reboot, throttling, crash under sustained load possible)

### Example DTS delta (for profile `mid`)
```dts
&cpu_opp_table {
    opp-1608000000 {
        opp-supported-hw = <0xf>;
    };
};
```

### Runtime cap (after boot)
- If higher OPP appears, cap with cpufreq to stage rollout:
```sh
uci set cpufreq.cpufreq.governor0='schedutil'
uci set cpufreq.cpufreq.minfreq0='864000'
uci set cpufreq.cpufreq.maxfreq0='1608000'   # or 1512000/1800000
uci commit cpufreq
/etc/init.d/cpufreq restart
```

### Validation checklist
```sh
cat /sys/devices/system/cpu/cpufreq/policy0/scaling_available_frequencies
cat /sys/devices/system/cpu/cpufreq/policy0/scaling_max_freq
cat /sys/class/thermal/thermal_zone*/temp
logread | grep -Ei 'thermal|throttle|watchdog|panic|reset|oom'
```

### Source references
- LKML/OpenWrt patch lineage for IPQ6018 freq extension to 1.5GHz:
  - https://www.spinics.net/lists/arm-kernel/msg1060556.html
  - https://lists.infradead.org/pipermail/lede-commits/2024-July/021785.html
- QCOM cpufreq nvmem speed-bin policy context:
  - https://www.openwall.com/lists/kernel-hardening/2019/01/10/7
- Community AX5/AX1800 OC practice (non-official, for trend reference only):
  - https://www.right.com.cn/forum/thread-8345075-1-1.html
  - https://www.xxs2.com/2533.html
