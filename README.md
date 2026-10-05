# CMFD miners

Precompiled, optimized Common Foundry (CMFD) GPU miners for Windows and Linux.
Community distribution for [CMFD Pool](https://cmfd-pool.online/).

## Downloads

Release **r13** — October 5, 2026. Binaries are hosted in GitHub Releases.

| Platform | Package |
| --- | --- |
| Windows x64 | [Download ZIP](https://github.com/lucasan123/cmfd-miners/releases/download/r13/cmfd-miner-mainnet-windows.zip) |
| Linux x64 | [Download tar.gz](https://github.com/lucasan123/cmfd-miners/releases/download/r13/cmfd-miner-mainnet-linux.tar.gz) |
| HiveOS Jammy 22.04+ | [Custom miner package 13.0.0](https://github.com/lucasan123/cmfd-miners/releases/download/hiveos-13.0.0/cmfd-miner-hiveos-13.0.0.tar.gz) |
| Checksums | [SHA256SUMS](https://github.com/lucasan123/cmfd-miners/releases/download/r13/SHA256SUMS) |

Packages include the miner, launch scripts, model downloader, checksums and an English README. No wallet or private keys are included.

## Set your wallet address

**In the Windows/Linux launchers, replace the preset operator address with YOUR CMFD mainnet public receiving address. If you leave the preset address unchanged, you will mine for its owner.** HiveOS uses the wallet from your Flight Sheet and has no preset payout address. Your address contains 64 hexadecimal characters. Never enter a seed, private key or wallet password.

### Windows

1. Extract the complete ZIP.
2. Run `start-windows.bat`.
3. Paste your address at **Your mainnet public wallet address**, then enter a rig name.
4. To save your address, edit the `PAYOUT` value in `start-windows.bat`.

### Linux

Extract the complete archive, open a terminal in that folder, then run:

```sh
chmod +x cmfd-miner-v4 start-linux.sh download-model.sh
sha256sum -c SHA256SUMS
./download-model.sh
./start-linux.sh YOUR_PUBLIC_ADDRESS rig-name
```

Replace `YOUR_PUBLIC_ADDRESS` and `rig-name`. To save your address, set `CMFD_PAYOUT` or edit the default `PAYOUT` in `start-linux.sh`.

### HiveOS

Use the [dedicated HiveOS release and Flight Sheet instructions](https://github.com/lucasan123/cmfd-miners/releases/tag/hiveos-13.0.0).
This package contains the qualified r13 binary with HiveOS configuration,
launch and statistics adapters.

| Flight Sheet field | Value |
| --- | --- |
| Miner | Custom |
| Miner name | `cmfd-miner-hiveos` |
| Installation URL | `https://github.com/lucasan123/cmfd-miners/releases/download/hiveos-13.0.0/cmfd-miner-hiveos-13.0.0.tar.gz` |
| Hash algorithm | `forgematrixv4` |
| Wallet and worker template | `%WAL%` |
| Pool URL | `cmfd+tls://109.199.124.187:29465?pin=ebe88f5e05f3a222208d551d05b6d39057b64ce8239ba7a708d487e15ac711be` |
| Pass | `%WORKER_NAME%` |
| Extra config arguments | Empty; optionally `--gpu 0,1 --batch 4` |

Requires **Jammy/Ubuntu 22.04 or newer (glibc 2.34+)**, an **x86-64/SSE2 CPU (AVX2 optional)**
and **NVIDIA R580+**. Focal/Ubuntu 20.04 is not supported by this build.
The CUDA toolkit is not needed. All NVIDIA GPUs are selected by default.
The verified model downloads automatically and remains outside the miner
installation directory across upgrades. Keep the hashrate watchdog disabled
during the first download and model load.

HiveOS displays the real **total rig FW/s** as H/s. Per-GPU rates are unavailable
in r13 and are left empty. The adapter was tested on Ubuntu 22.04 and with the
HiveOS client statistics code; no complete HiveOS GPU session is claimed.
[HiveOS checksums](https://github.com/lucasan123/cmfd-miners/releases/download/hiveos-13.0.0/SHA256SUMS)
and the full English README are included in the release.

## Requirements and operation

- Compatible NVIDIA GPU and driver; Windows x64 or Linux x64.
- At least 15 GB of free disk space for the model download and assembly.
- All NVIDIA GPUs are selected by the launchers automatically.
- `MODEL-V2.bank` is downloaded separately (6,442,975,416 bytes); its parts and assembled file are checked with SHA-256.
- Keep the miner open while it waits for work. It reconnects and resumes automatically.
- Pool fee: **3%**. PPLNS rewards mature after **100 confirmations**; automatic payments start at **10 CMFD**.
- Leave the consensus fingerprint set to `auto`. It is not your wallet address.

## r13 CPU compatibility

AVX2 is now optional. CPU code and every dependency are rebuilt for baseline
x86-64/SSE2, with automatic SIMD selection in BLAKE3. The optimized r12 GPU
kernels and pool protocol are retained. A physical Celeron G5905 without AVX
or AVX2 reproduced r12's illegal-instruction failure; r13 passes CPU, TLS,
pool and verifier tests and mines with RTX 3060. The HiveOS preflight accepts it.

On RTX 4090 at 500 W and batch 4, alternating r12/r13/r12/r13 measured
**79.38 / 79.68 FW/s**, respectively. This small difference is not a speed
upgrade claim. The Celeron performance benchmark was not completed.

Run `cpu-diagnostics.bat` or `bash cpu-diagnostics.sh` to create
`CPU-DIAGNOSTICS.txt` without using the GPU or connecting to a pool.
See the [r13 test coverage and limitations](https://github.com/lucasan123/cmfd-miners/releases/tag/r13).

## Historical r12 benchmarks

The measurements below belong to r12 and are retained as historical context.

Paired Linux measurements on October 5, 2026. **FW/s is completed work per
second**, not an earnings estimate. Authentic model, stable local TLS job,
complete GPU computation, output transfer and BLAKE3 hashing. Same host,
driver and power limit for each r11/r12 comparison.

### Batch 16, stable job

Alternating r11/r12/r11/r12, 100 seconds each. First 30-second reporting
window excluded; mean of the remaining windows and both repetitions.

| GPU | Power limit | r11 FW/s | r12 FW/s | Change |
|---|---:|---:|---:|---:|
| RTX 5070 Ti | 300 W | 45.14 | **47.10** | +4.3% |
| RTX 4090 | 450 W | 69.86 | **76.92** | +10.1% |
| RTX 5090 | 575 W | 91.05 | **93.80** | +3.0% |

### Default batch 4 and changing jobs

One 100-second pair at batch 4 measured 42.78→44.55 FW/s on 5070 Ti and
68.10→75.75 on 4090, and 82.81→85.56 on 5090. The default stays at **4**: in ten job changes spaced
5.137 seconds apart, the 5070 Ti candidate discarded 8 nonces per change,
versus 32 at batch 16. Maximum old-batch retirement was166 ms versus653 ms.
This small sample is not a p99 latency estimate.

Real rates depend on job changes, pool synchronization, clock/power limits
and host configuration. The pool dashboard estimates credited work and can
differ from the miner's reported rate. No payable mainnet shares were sent
in these controlled tests. Other models in the RTX 40/50 families have not
been benchmarked with r12.

### Power and diagnostics

On the tested 5070 Ti, r12 measured 43.96 FW/s at 250 W and 47.10 at 300 W.
The tested 4090 measured 80.06 at 500 W and 84.76 at 600 W: about 5.9% more work
for 20% more power. Higher power reduced work per watt in these measurements.
**The miner does not change power limits or clocks.**

The Windows ZIP includes `diagnostica-potenza.bat`. Run it while mining to
save 60 seconds of GPU power, limits, clocks, temperature and process CPU.
It changes no settings and does not stop the miner.

### Correctness and platform coverage

Linux r12 passed official 50/50 vectors, two complete CPU/GPU nonce comparisons,
the 402,653,184-value trace, TLS/pool and persistent-verifier tests on 5070 Ti,
4090 and 5090. Windows passed CPU vectors and TLS tests; its sm89/sm120 device
instructions and encodings match Linux exactly. **Native Windows GPU execution
has not been tested for r12.** Keep r11 available as a rollback.

## Verify the download

Windows PowerShell:

```powershell
Get-FileHash .\cmfd-miner-mainnet-windows.zip -Algorithm SHA256
```

Linux:

```sh
sha256sum cmfd-miner-mainnet-linux.tar.gz
```

Expected archive SHA-256 values:

```text
e237d50273111d9d643728c49f2bf5d45ab434b5b71c9ce881ccb310b4a6d327  cmfd-miner-mainnet-windows.zip
bda8e21f1490c92a2b6f59d424b83eaefc9cecce888237582ae20c860ef1c829  cmfd-miner-mainnet-linux.tar.gz
```

The previous [r12 release](https://github.com/lucasan123/cmfd-miners/releases/tag/r12) remains available. This repository distributes binaries and documentation only; it is not the official Common Foundry project.
