# KebabCPU

XNU IOCPU driver for the **OnePlus 8T (kebab, SM8250)**.

It publishes one `IOCPU` nub per `/cpus` entry in the device tree and starts
secondary cores through **PSCI `CPU_ON`** over SMCCC (`hvc` or `smc`, chosen by
the `psci` device-tree node). The `cpus=N` boot-argument caps how many CPUs are
registered, which is how the first bring-up images stay on the boot CPU.

| Item | Value |
| --- | --- |
| Module type | `driver` (`drivers/KebabCPU`) |
| Machine config | `KEBAB`, 16 KiB pages, plain `arm64` (no PAC) |
| Kernel | xnu-8796 (iOS 16 generation) |
| Derived from | the negroni port (OnePlus 10 Pro, SM8450) |
| Reference plist | `KebabCPU.plist` |

## Device-tree contract

```
/cpus/cpu@N     reg = MPIDR (0x000, 0x100, ... 0x700 on SM8250)
                timebase-frequency = 19200000
/psci           compatible = "arm,psci-1.0", method = "smc"
```

The GIC redistributor lookup and SGI targeting use the real MPIDR affinity:
SM8250 puts the core number in **Aff1**, not Aff0.

## Status

The kernel builds and the driver is linked; secondary bring-up is untested on
hardware yet because the first images run with `cpus=1`.
