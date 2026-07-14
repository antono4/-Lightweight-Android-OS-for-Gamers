# NOVA OS — Quick Reference Card
## Executive Summary for Development Teams

---

## 🎯 Target Metrics

| Metric | Current (Stock) | NOVA OS Target | Improvement |
|--------|----------------|----------------|-------------|
| RAM Idle | ~3.5 GB | < 1.8 GB | **50% reduction** |
| Boot Time | ~25s | < 15s | **40% faster** |
| Touch Latency | ~40ms | < 20ms | **2x faster** |
| Storage Size | ~3.5 GB | ~800 MB | **77% smaller** |
| FPS Stability | 45-60fps (thermal) | 60fps locked | **Zero drops** |

---

## 🔧 Recommended Source Base

```
PRIMARY:  LineageOS 21.0 (Android 14)
├── Reason: Modular, documented, community support
├── Tree:   lineage-21.0
└── Device: Extract blobs from stock ROM

ALTERNATIVE: ArrowOS 14 (for ultra-light)
```

---

## 🗑️ Critical Debloat List (Quick Reference)

### MUST REMOVE (Safe)
```bash
# Google Bloat
com.google.android.apps.photos
com.google.android.videos
com.google.android.apps.youtube.music
com.google.android.apps.tachyon
com.google.android.printservice

# OEM Bloat
com.sec.android.app.samsungapps    # Galaxy Store
com.sec.android.app.bixby          # Bixby
com.samsung.android.mobileservice   # Samsung Pay/Member

# Analytics (Privacy + Perf)
com.google.android.gms.measurement
com.android.traceur
```

### MUST DISABLE (Don't Remove)
```bash
# Keep files, disable at runtime
flipboard.boxer.content
com.google.android.gms.backup
com.google.android.gms.phenotype
```

---

## ⚡ Memory Optimization Quick Config

### ZRAM Setup
```
Size: 50% of RAM
Algorithm: LZ4HC (better ratio than LZ4)
Swappiness: 180 (game mode: 60)
```

### LMKD Thresholds
```
Critical: 1024 KB
Target: 8192 KB
Adj: 0,100,200,300,900,906,9000
```

---

## 🎮 Game Mode Quick Settings

| Parameter | Value | File/Path |
|-----------|-------|-----------|
| CPU Governor | performance | /sys/.../scaling_governor |
| Touch Rate | 1000 Hz | /sys/class/touchscreen/.../sample_rate |
| Thermal Limit | 45°C | /sys/class/thermal/... |
| Memory Lock | 4096 MB | /dev/cpuset/foreground/boost |

---

## 📷 Creator Suite Requirements

### Camera2 API Level
```
FULL (Level 3) Support Required:
├── android.request.maxNumOutputStreams = 5
├── android.request.availableToneMapModes = [1,2,3,4,5]
├── android.request.availableFaceDetectModes = FULL
└── HAL Version: 3.5 minimum
```

### Screen Recorder Specs
```
Video:  H.264/H.265, up to 4K@60fps, 50Mbps
Audio:  320kbps, internal + mic options
Format: MP4 (H.264) or MKV (H.265)
```

### I/O Scheduler
```
Primary Storage: mq-deadline (low latency)
SD Card: bfq (sequential throughput)
Queue Depth: 64
Read-ahead: 128KB
```

---

## 🔧 Build Commands

```bash
# Setup
./setup-build-env.sh

# Sync
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0
repo sync -c -j$(nproc)

# Build
source build/envsetup.sh
breakfast <device>
mka bacon

# NOVA Build
mka nova
```

---

## 📁 Key Files to Modify

| File | Purpose | Priority |
|------|---------|----------|
| `/system/build.prop` | System properties | HIGH |
| `/system/etc/lmkd.conf` | Memory killer config | HIGH |
| `frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java` | App lifecycle | HIGH |
| `kernel/sched/cpufreq_schedutil.c` | CPU governor | MEDIUM |
| `drivers/input/touchscreen/*.c` | Touch driver | MEDIUM |

---

## 📊 Size Comparison

```
┌────────────────────────────────────────────┐
│  STOCK ROM vs NOVA OS                      │
├────────────────────────────────────────────┤
│                                            │
│  Stock: ████████████████████ 3.5 GB        │
│  NOVA:  ████ 800 MB                        │
│                                            │
│  Savings: 2.7 GB (77%)                     │
│                                            │
└────────────────────────────────────────────┘
```

---

*Quick Reference Card v1.0*
