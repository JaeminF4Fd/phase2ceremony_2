# Hermez First Phase 2 ceremony attestation

**Contribution Number:** 0003

**Name:** Koh Wei Jie

**Date:** Feb 13th, 2021

**Location:** The cloud

**Device:** Google Cloud VM n2d-standard-2 (2 vCPUs, 8 GB memory)
Confidential VM service enabled

I used a Google Cloud account enrolled in [Google's Advanced Protection
Program](https://landing.google.com/advancedprotection/). The Confidential
Computing option purports to provide encryption to data in the cloud while it
is being processed. I can't speak to whether this service is truly secure, but
generally, the more diverse the hardware/VMs used, the more secure the
ceremony. As such, even if Confidential Computing is only as secure than a
regular Google Cloud VM, an adversary will have the already difficult task of
compromising Google Cloud in addition to compromising every other participant's
device.

**Entropy sources:** Hex values from [the Australian
National University's Quantum Random Number
Generator](https://qrng.anu.edu.au/random-hex/) and the output of `python3 -c
'import secrets; print(secrets.token_hex(40960))'` from my laptop.

`lscpu` output:

```
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   48 bits physical, 48 bits virtual
CPU(s):                          2
On-line CPU(s) list:             0,1
Thread(s) per core:              2
Core(s) per socket:              1
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       AuthenticAMD
CPU family:                      23
Model:                           49
Model name:                      AMD EPYC 7B12
Stepping:                        0
CPU MHz:                         2249.998
BogoMIPS:                        4499.99
Hypervisor vendor:               KVM
Virtualization type:             full
L1d cache:                       32 KiB
L1i cache:                       32 KiB
L2 cache:                        512 KiB
L3 cache:                        16 MiB
NUMA node0 CPU(s):               0,1
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Full AMD retpoline, IBPB conditional, IBRS_FW, STIBP conditional, RSB filling
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_
                                 good nopl nonstop_tsc cpuid extd_apicid tsc_known_freq pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm cmp_leg
                                 acy cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw topoext ssbd ibrs ibpb stibp vmmcall fsgsbase tsc_adjust bmi1 avx2 smep bmi2 rdseed adx smap clflushopt clw
                                 b sha_ni xsaveopt xsavec xgetbv1 clzero xsaveerptr arat npt nrip_save umip rdpid
```

I downloaded the following files and checked that their Blake2 hashes match
those attested by the previous participant.

```
circuit-1960-32-256-64_hez2_0002.zkey
3c3ca2922f8c34d5c99f9ed6cfc33f0eda396d39df21578ee9ba5125e59d2672f5b3f047b278d2ea0ac5730ae685f4a7a03d529f891e5641dcdd8b7b7f66bb5d

circuit-352-32-256-64_hez2_0002.zkey
cb0238969fc19071191627b24505247ff3052b73325beb18d60e8b3d72d0667f6d97391864223f585b93cac1cebfb4087af63bdd59b854f7baee8fbc0301b2f5

withdraw_hez2_0002.zkey
ad630f7b3f061c461400d5993d0e8d6914cc4ec5b681ae2927459dd00df089379ff1c477a7675cb6b21277876644ecf3b82de736c9549bf8ecf4ee717166aab4
```

## My contributions

```
snarkjs zkey contribute withdraw_hez2_0002.zkey withdraw_hez2_0003.zkey -v -n=weijie
[DEBUG] snarkJS: Applying key: L Section: 0/69148
[DEBUG] snarkJS: Applying key: L Section: 65536/69148
[DEBUG] snarkJS: Applying key: H Section: 0/131072
[DEBUG] snarkJS: Applying key: H Section: 65536/131072
[INFO]  snarkJS: Circuit Hash: 
                ec15023b 4b03e104 a8b8f620 e48b6f8a
                f6231eeb d3a3e47c c7745379 34b16062
                301a0ad3 91d56c7e cf98b8d3 28f231f5
                42f67e89 893e2b21 277e7019 55020b8c
[INFO]  snarkJS: Contribution Hash: 
                f48022f6 6ffd792f 88f06d5b c59eb834
                de2176ca cfc9cdf0 e04d9a9a 55d69a9a
                bdc7e429 d1f26f88 13e1cae5 ca92a059
                8c0e5ed7 bb1bf9da 4555bb12 ec545497
```

```
snarkjs zkey contribute circuit-352-32-256-64_hez2_0002.zkey circuit-352-32-256-64_hez2_0003.zkey -v -n=weijie
[INFO]  snarkJS: Circuit Hash: 
                c356c4af cc7b59cc 42228f92 322a7ce6
                015dd4f2 e3ab3871 fbdfaf81 fb493954
                1f701c6f 8dd93a18 03942a44 3f79aa2f
                65387a88 f828f4db 710194bb 18da2437
[INFO]  snarkJS: Contribution Hash: 
                5cbd2a0e ec404cef c104c361 d1e41508
                b6aa41fa fb499b63 08c7757e ae1fcb1e
                8df64a82 b43fa2d9 836716ad 56fd875f
                6af3dbb1 b451d27b 42e2f985 ef56b2c0
```

```
snarkjs zkey contribute circuit-1960-32-256-64_hez2_0002.zkey circuit-1960-32-256-64_hez2_0003.zkey -v -n=weijie
[INFO]  snarkJS: Circuit Hash: 
                755a1c7b e3556941 1c368f6a f8700769
                9015c263 8864499a e3051d5e 5160eea4
                af4a8a28 1ab233f0 f7b4dc65 f51ad0aa
                0b495c29 54f6b493 a04fdf7a 58ecff17
[INFO]  snarkJS: Contribution Hash: 
                7e293790 7681733f 4b9eb881 fea10668
                c47b49db 348234b9 1af9e32d a3be1d8d
                842f324d ab09b77f 79007409 db004fcf
                2026c4dd be508eac f8c01d49 65a5cb2b
```
