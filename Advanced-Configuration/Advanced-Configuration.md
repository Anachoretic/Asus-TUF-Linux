## 1. CPU boost:
CPU boost is a feature that temporarily raises a processor’s clock speed above its base frequency to improve performance during demanding tasks. Disabling it 
causes the CPU to stay near its base clock, which reduces temperatures but also lowers performance. Applications that rely on high CPU speeds such as games or other CPU-intensive workloads will see a 
noticeable performance drop, while lighter tasks will be mostly unaffected. You can disable boost if you don’t use such applications or if you are fine with the performance and temperature tradeoff.

```bash
echo 0 > /sys/devices/system/cpu/cpufreq/boost
```

To enable CPU Boost again, simply echo `1` instead of `0`.

## 2. GPU Undervolt:

