# CMFD miners

Precompiled, optimized Common Foundry (CMFD) GPU miners for Windows and Linux.
Community distribution for [CMFD Pool](https://cmfd-pool.online/).

## Downloads

Release **r11** — October 3, 2026. Binaries are hosted in GitHub Releases.

| Platform | Package |
| --- | --- |
| Windows x64 | [Download ZIP](https://github.com/lucasan123/cmfd-miners/releases/download/r11/cmfd-miner-mainnet-windows.zip) |
| Linux x64 | [Download tar.gz](https://github.com/lucasan123/cmfd-miners/releases/download/r11/cmfd-miner-mainnet-linux.tar.gz) |
| Checksums | [SHA256SUMS](https://github.com/lucasan123/cmfd-miners/releases/download/r11/SHA256SUMS) |

Packages include the miner, launch scripts, model downloader, checksums and an English README. No wallet or private keys are included.

## Set your wallet address

**Replace the preset operator address with YOUR CMFD mainnet public receiving address. If you leave the preset address unchanged, you will mine for its owner.** Your address contains 64 hexadecimal characters. Never enter a seed, private key or wallet password.

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

## Requirements and operation

- Compatible NVIDIA GPU and driver; Windows x64 or Linux x64.
- At least 15 GB of free disk space for the model download and assembly.
- All NVIDIA GPUs are selected by the launchers automatically.
- `MODEL-V2.bank` is downloaded separately (6,442,975,416 bytes); its parts and assembled file are checked with SHA-256.
- Keep the miner open while it waits for work. It reconnects and resumes automatically.
- Pool fee: **3%**. PPLNS rewards mature after **100 confirmations**; automatic payments start at **10 CMFD**.
- Leave the consensus fingerprint set to `auto`. It is not your wallet address.

## Benchmarks

Recorded Linux measurements from October 3, 2026. **FW/s means completed work per second.** It is not a coin earnings estimate.

### Stable-job benchmark

Authentic `MODEL-V2.bank`, local TLS pool with a stable job, batch size **16**, complete GPU computation and BLAKE3 output hashing. Mean of 10-second reporting windows after the initial loading window:

| GPU | GPUs | Total FW/s |
| --- | ---: | ---: |
| RTX 5070 Ti | 1 | **43.44** |
| RTX 4090 | 1 | **64.62** |
| RTX 5090 | 1 | **87.91** |
| RTX 5090 | 2 | **174.25** |

These are measurements of the optimized V4 qualification builds, not fresh r11 benchmarks. The 4090 result used the intermediate qualification build; its GPU machine code matched the final qualification build. These tests did not submit payable mainnet shares. No Windows GPU performance measurement is claimed.

### Live mining observations

Short r10 canary observations while work was continuously available, using the launcher's default batch size **4**:

| GPU configuration | Total reported FW/s |
| --- | ---: |
| 4 × RTX 5060 Ti | approximately **89.5** |
| 1 × RTX 5090 | approximately **75.5** |

These short samples are not sustained benchmarks or a prediction of r11 performance. Actual wall-clock throughput can be lower because of job changes, synchronization pauses, clock/power limits and host configuration. The dashboard's share-based estimate measures credited work and can differ from the miner's reported rate. The r11 launcher defaults to batch size 4 to reduce discarded work on changing jobs.

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
2876b96710729b356781825a81cf3f3778f0e21fb7eabbae214ee78211216072  cmfd-miner-mainnet-windows.zip
af94dea5b405ef14901ee005a74ceb15adc038586b196812ec39e4580be64b4c  cmfd-miner-mainnet-linux.tar.gz
```

These are the same r11 packages previously offered by the pool dashboard. This repository distributes binaries and documentation only; it is not the official Common Foundry project.
