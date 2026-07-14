# NOVA OS — Ultra-Light Custom ROM Blueprint
## Specification Document v1.0
### "Performance First, Power Always"

---

## 1. Executive Summary

**NOVA OS** adalah custom ROM Android ultra-ringan yang dirancang khusus untuk gamer hardcore dan content creator. Target utama adalah konsumsi RAM minimum (< 2GB saat idle), responsivitas tertinggi, dan performa grafis premium dengan tetap mempertahankan fitur lengkap yang diperlukan untuk produktivitas kreatif.

### Design Philosophy
```
┌─────────────────────────────────────────────────────────────┐
│                    NOVA OS PRINCIPLES                        │
├─────────────────────────────────────────────────────────────┤
│  1. ZERO-BLOAT      : Hapus semua komponen tidak esensial  │
│  2. AGGRESSIVE-OPT  : Optimasi agresif pada level kernel   │
│  3. GAMER-FIRST     : Resource priority untuk gaming       │
│  4. CREATOR-POWER   : Hardware access penuh untuk media    │
│  5. LIGHTWEIGHT     : Target < 2GB RAM idle, < 5GB storage │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Architecture Overview

### 2.1 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                         NOVA OS LAYER STACK                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    USER EXPERIENCE LAYER                      │   │
│  │  ┌────────────┐  ┌─────────────┐  ┌────────────────────┐    │   │
│  │  │ Nova Lau-  │  │  Game Space │  │ Creator Studio     │    │   │
│  │  │ ncher     │  │  Dashboard  │  │ (Screen Recorder)  │    │   │
│  │  └────────────┘  └─────────────┘  └────────────────────┘    │   │
│  ├──────────────────────────────────────────────────────────────┤   │
│  │                      FRAMEWORK LAYER                          │   │
│  │  ┌──────────────────────┐  ┌────────────────────────────┐   │   │
│  │  │ Optimized Activity   │  │ Custom Resource Manager    │   │   │
│  │  │ Manager Service      │  │ (Dynamic RAM Allocation)   │   │   │
│  │  └──────────────────────┘  └────────────────────────────┘   │   │
│  │  ┌──────────────────────┐  ┌────────────────────────────┐   │   │
│  │  │ Nova Thermal Service  │  │ Touch Input Optimizer      │   │   │
│  │  └──────────────────────┘  └────────────────────────────┘   │   │
│  ├──────────────────────────────────────────────────────────────┤   │
│  │                    ANDROID RUNTIME                            │   │
│  │  ┌────────────┐  ┌─────────────┐  ┌────────────────────┐    │   │
│  │  │ ART (OAT)  │  │ Bionic Lib  │  │ Nova Power Manager │    │   │
│  │  │ Optimized  │  │ Optimized   │  │                     │    │   │
│  │  └────────────┘  └─────────────┘  └────────────────────┘    │   │
│  ├──────────────────────────────────────────────────────────────┤   │
│  │                    HARDWARE ABSTRACTION                       │   │
│  │  ┌────────────┐  ┌─────────────┐  ┌────────────────────┐    │   │
│  │  │ GPU Drv    │  │ Audio HAL   │  │ Camera2 Full API   │    │   │
│  │  │ Tuned      │  │ Low-Latency │  │ Level 3 Support    │    │   │
│  │  └────────────┘  └─────────────┘  └────────────────────┘    │   │
│  ├──────────────────────────────────────────────────────────────┤   │
│  │                    LINUX KERNEL LAYER                         │   │
│  │  ┌────────────────────────────────────────────────────────┐  │   │
│  │  │  • ZRAM Config (2:1 Compression)                       │  │   │
│  │  │  • LMKD Tuned (Aggressive OOM Adj)                     │  │   │
│  │  │  • Custom I/O Scheduler (Maple/BFQ)                    │  │   │
│  │  │  • Touch Driver Patched (1000Hz Sampling)             │  │   │
│  │  │  • Thermal Governor: SCHEDUTIL + Nova Profile          │  │   │
│  │  │  • CPU Governor: Schedutil Performance                 │  │   │
│  │  └────────────────────────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

### 2.2 Component Dependency Graph

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COMPONENT DEPENDENCY MAP                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Game Space Controller ─────┬──────► CPU Governor Switch          │
│           │                  │              │                        │
│           │                  │              ▼                        │
│           ▼                  │      ┌────────────────┐               │
│   Notification Blocker ───────┼──────► Doze Mode Override            │
│           │                  │              │                        │
│           │                  │              ▼                        │
│           ▼                  │      ┌────────────────┐               │
│   Background Sync Killer ────┴──────► Sync Manager                  │
│                                     └────────────────┘               │
│                                                                      │
│   ──────────────────────────────────────────────────────────────    │
│                                                                      │
│   Memory Manager ────────────┬──────► ZRAM Device                   │
│           │                  │              │                        │
│           ▼                  │              ▼                        │
│   LMKD Config ───────────────┼──────► /sys/module/lowmemorykiller   │
│           │                  │              │                        │
│           ▼                  │              ▼                        │
│   App Lifecycle Manager ─────┴──────► Activity Manager Service      │
│                                                                      │
│   ──────────────────────────────────────────────────────────────    │
│                                                                      │
│   Creator Studio ─────────────┬──────► MediaRecorder API            │
│           │                   │              │                        │
│           ▼                   │              ▼                        │
│   Audio Pipeline ─────────────┴──────► AAudio / Oboe HAL             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3. Recommended Source Code Base

### 3.1 Primary Source Options

| Base ROM | Pros | Cons | Recommendation |
|----------|------|------|----------------|
| **LineageOS 21** | Stabil, dokumentasi lengkap, komunitas besar | Bloat masih cukup banyak | ⭐ **Recommended** |
| **Pixel Experience** | Dekat dengan vanilla Android, update cepat | Kurang customizable | Good Alternative |
| **Evolution X** | Banyak fitur tambahan, performa bagus | Overhead fitur berlebihan | Untuk reference |
| **ArrowOS** | Ringan, modular | Komunitas lebih kecil | Advanced option |
| **AOSP 14** | Murni, kontrol penuh | Mulai dari nol | Expert only |

### 3.2 Recommended Architecture Choice

```
┌─────────────────────────────────────────────────────────────┐
│               RECOMMENDED: LineageOS 21 Base                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   Base: LineageOS 21 (Android 14)                           │
│   ├── Tree: lineage-21.0                                    │
│   ├── Device Tree: <specific-device>                        │
│   ├── Vendor: Proprietary blobs dari stock ROM             │
│   └── Kernel: Stock kernel dengan patch kustom             │
│                                                              │
│   Mengapa LineageOS?                                         │
│   ├── Modular architecture (easy debloat)                    │
│   ├── Well-documented build process                         │
│   ├── Large community support                               │
│   ├── Clean codebase (minimal proprietary)                  │
│   └── Frequent security patches                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. Component Removal Specification (Debloat List)

### 4.1 System Apps to Remove (CRITICAL)

```bash
# ============================================================================
# NOVA OS DEBLOAT SCRIPT - COMPLETE REMOVAL LIST
# ============================================================================

# --------------------------------------------------------------------
# GOOGLE BLOATWARE (Safe to Remove)
# --------------------------------------------------------------------
PACKAGES_TO_REMOVE=(
    # Basic Apps
    "com.google.android.apps.photos"           # Photos (bisa diganti GCam)
    "com.google.android.apps.tachyon"          # Duo
    "com.google.android.apps.youtube.music"    # YouTube Music
    "com.google.android.videos"                # Movies & TV
    "com.google.android.printservice"          # Print Service
    "com.google.android.partnersetup"          # Partner Setup
    "com.google.android.onetimeinitializer"    # One-time Init
    "com.google.android.setupwizard"           # Setup Wizard (custom)
    "com.google.android.gms"                   # GMS Core (PARTIAL - keep stubs)
    
    # Android System Apps
    "com.android.traceur"                      # Systrace
    "com.android.phone.automation"             # Phone Automation
    "com.android.wallpaper.livepicker"         # Live Wallpapers
    "com.android.providers.partnerbookmarks"   # Partner Bookmarks
    
    # Samsung Specific (if applicable)
    "com.sec.android.app.samsungapps"          # Galaxy Store
    "com.sec.android.app.bixby"                # Bixby
    "com.sec.android.app.sbrowser"              # Samsung Browser
    "com.sec.android.app.clipboard"            # Samsung Clipboard
    "com.sec.android.app.kids"                 # Samsung Kids
    "com.samsung.android.app.social"            # Samsung Social
    "com.samsung.android.mobileservice"        # Samsung Pay/Member
    "com.samsung.android.stickerplugin"         # AR Stickers
    "com.samsung.android.app.camera.sticker.facear"  # AR Emoji
    
    # Carrier Bloat
    "com.verizon.provision"                     # Verizon Setup
    "com.t-mobile.pr.mpretraining"             # T-Mobile
    "com.att.candp"                            # AT&T
    "com.asus.aura"                            # ASUS
    "com.miui.android.familycloud"             # Xiaomi Cloud
)
```

### 4.2 System Services to Disable (NOT REMOVE - Only Disable)

```bash
# ============================================================================
# SERVICES TO DISABLE (Keep files, disable at runtime)
# ============================================================================

# --------------------------------------------------------------------
# BOOT LOADER DISABLED SERVICES
# --------------------------------------------------------------------
DISABLED_SERVICES=(
    # Connectivity Services
    "flipboard.boxer.content"                  # News/RSS aggregator
    "com.google.android.gms.update"            # Auto-update service
    "com.google.android.configupdater"         # Config update
    
    # Background Sync
    "com.google.android.gms.backup"            # Cloud Backup
    "com.google.android.gms.phenotype"         # Config sync
    "android.autos Marquand"                    # (disabled for gaming)
    
    # Analytics (Privacy + Performance)
    "com.google.android.gms.measurement"       # Analytics
    "com.google.android.gms.usage"             # Usage reporting
    
    # OEM Services
    "com.samsung.android.securitylogagent"     # Security logging
    "com.samsung.android.app.watchman"         # Samsung tracking
    "com.miui.system"                         # MIUI system services
)

# --------------------------------------------------------------------
# PM DISABLE COMMANDS
# --------------------------------------------------------------------
for pkg in "${DISABLED_SERVICES[@]}"; do
    echo "Disabling: $pkg"
    pm disable-user --user 0 "$pkg" 2>/dev/null || true
done
```

### 4.3 Filesystem Cleanup (System Partition)

```bash
# ============================================================================
# FILESYSTEM CLEANUP PATHS
# ============================================================================

# --------------------------------------------------------------------
# REMOVE UNUSED FONTS (Save ~50MB)
# --------------------------------------------------------------------
/system/fonts/NotoSansShavian*
/system/fonts/NotoSansSumerian*
/system/fonts/NotoSerif*
/system/fonts/DroidSansFallback*    # Non-latin fallback (keep minimal)
/system/fonts/Roboto*.xml           # Legacy XML configs

# --------------------------------------------------------------------
# REMOVE UNUSED LANGUAGES (Keep EN, ID, AR only - Save ~200MB)
# --------------------------------------------------------------------
/system/app/LatinImeDictionaryLookup
/system/usr/srec/                   # Speech recognition (keep for now)
/system/usr/srec/config/           # Keep only en-US

# --------------------------------------------------------------------
# REMOVE UNUSED LIBRARIES (Save ~100MB)
# --------------------------------------------------------------------
/system/lib64/libdrmdecrypt.so
/system/lib64/libsfplugin_ccm_odm.so
/system/lib/vndidlibskm.so
/system/lib/libdiag.so

# --------------------------------------------------------------------
# REMOVE PREBUILT APKS (After extraction)
# --------------------------------------------------------------------
/system/priv-app/GoogleHome/*       # Hub app
/system/priv-app/AndroidAuto*       # Android Auto stub
/system/app/WallpaperBackup         # Wallpaper backup
/system/app/WallpaperPicker*        # Use NOVA Wallpaper
```

---

## 5. Memory Optimization Architecture

### 5.1 ZRAM Configuration

```bash
# ============================================================================
# ZRAM CONFIGURATION - /system/etc/init.nova.zram.rc
# ============================================================================

on property:sys.boot_completed=1
    # ZRAM Configuration for NOVA OS
    # Target: 50% of RAM for swap, 2:1 compression ratio
    
    # Configure zram size (50% of available RAM)
    setprop persist.nova.zram.size $(expr $(cat /proc/meminfo | grep MemTotal | awk '{print $2}') / 2)
    
    # Initialize zram device
    write /sys/block/zram0/comp_algorithm lz4hc
    write /sys/block/zram0/disksize $(expr $(cat /proc/meminfo | grep MemTotal | awk '{print $2}') / 2)K
    
    # Set swappiness (aggressive for gaming)
    write /proc/sys/vm/swappiness 180
    
    # Enable zram
    swapon /dev/block/zram0

on property:sys.game.mode=1
    # GAME MODE: Reduce swappiness for active apps
    write /proc/sys/vm/swappiness 60
    
    # Lock game process in memory
    write /proc/sys/vm/watermark_boost_factor 0

on property:sys.game.mode=0
    # Return to normal
    write /proc/sys/vm/swappiness 180
```

### 5.2 LMKD (Low Memory Killer Daemon) Configuration

```bash
# ============================================================================
# LMKD CONFIGURATION - /system/etc/lmkd.conf
# ============================================================================

# NOVA OS LMKD Tuning
# Philosophy: Aggressive on background apps, gentle on foreground

# Memory thresholds (in KB)
# When available memory drops below these, kill apps by priority

# Critical threshold - Kill invisible apps immediately
ro.lmkd.critical=1
ro.lmkd.critical_min_free=1024        # 1MB critical reserve

# Foreground app minfree - Never kill current app
ro.lmkd.target_free_kbytes=8192       # Target 8MB free

# OOM score adjustments
ro.lmkd.adj=0,100,200,300,900,906,9000

# Minfree values (per-tier memory thresholds)
# Format: minfree[0],minfree[1],...,minfree[n]
# Each entry is the minimum free memory (in pages) for that tier
ro.lmkd.minfree=1536,2048,4096,8192,16384,32768

# Process priority to OOM score mapping
ro.lmkd.psi_stage=200                 # PSI stage threshold

# Enable memory pressure monitoring
ro.lmkd.use_psi_monitors=1

# Aggressive reclaim for gaming
ro.lmkd.thrashing_limit=50           # % CPU thrashing allowed
ro.lmkd.thrashing_limit_decay=10    # Decay rate
```

### 5.3 Kernel Memory Tuning

```bash
# ============================================================================
# KERNEL PARAMETERS - /sysctl.conf additions
# ============================================================================

# Virtual Memory Settings
vm.swappiness = 180                  # Aggressive swap (game mode: 60)
vm.vfs_cache_pressure = 50           # Less aggressive dentry reclaim
vm.dirty_ratio = 5                   # Start writing earlier
vm.dirty_background_ratio = 2
vm.overcommit_memory = 1             # Always allow overcommit
vm.max_map_count = 655360

# TCP Buffer Optimization
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216

# IPC Optimization
kernel.shmmax = 4294967296
kernel.shmall = 1073741824
```

---

## 6. Gamer Features Architecture

### 6.1 Game Space Dashboard

```
┌─────────────────────────────────────────────────────────────────────┐
│                      GAME SPACE ARCHITECTURE                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    GAME SPACE SERVICE                         │   │
│  │                                                              │   │
│  │  ┌─────────────┐  ┌──────────────┐  ┌───────────────────┐    │   │
│  │  │ Resource    │  │ Notification │  │ Thermal          │    │   │
│  │  │ Allocator   │  │ Controller   │  │ Manager          │    │   │
│  │  └──────┬──────┘  └──────┬───────┘  └─────────┬────────┘    │   │
│  │         │                │                     │              │   │
│  │         ▼                ▼                     ▼              │   │
│  │  ┌──────────────────────────────────────────────────────┐   │   │
│  │  │              GAME MODE ACTIVATOR                      │   │   │
│  │  │  • CPU Governor → Performance                         │   │   │
│  │  │  • GPU Clocks → Max                                   │   │   │
│  │  │  • Disable background sync                            │   │   │
│  │  │  • Lock game app in memory                           │   │   │
│  │  │  • Block incoming calls/notifications                │   │   │
│  │  └──────────────────────────────────────────────────────┘   │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 6.2 Game Mode Implementation

```java
// ============================================================================
// NOVA GAME SPACE MANAGER - com.nova.gamespace.GameModeManager
// ============================================================================

package com.nova.gamespace;

import android.app.ActivityManager;
import android.app.NotificationManager;
import android.content.Context;
import android.os.Build;
import android.os.PowerManager;
import android.os.SystemProperties;
import android.os.CpuBoostManager;

public class GameModeManager {
    private static final String TAG = "NovaGameMode";
    private static GameModeManager sInstance;
    
    // Game Mode States
    public static final int MODE_NORMAL = 0;
    public static final int MODE_GAME = 1;
    public static final int MODE_BATTLE = 2;  // Ultra performance
    
    // CPU/GPU Profiles
    private static final String PROP_CPU_GOVERNOR = "sys.nova.cpu.governor";
    private static final String PROP_GPU_PERF = "sys.nova.gpu.perf";
    private static final String PROP_TOUCH_SAMPLE = "sys.nova.touch.rate";
    
    public static class GameProfile {
        public int cpuMinFreq;        // MHz
        public int cpuMaxFreq;         // MHz
        public int gpuPerfLevel;      // 0-100
        public int touchSampleRate;   // Hz
        public int thermalLimit;      // °C threshold
        public int memoryLock;        // MB to lock
        
        public static GameProfile ULTRA = new GameProfile(
            /*cpuMin*/ 1800, /*cpuMax*/ 3200, /*gpu*/ 100,
            /*touch*/ 1000, /*thermal*/ 45, /*memLock*/ 4096
        );
        
        public static GameProfile BALANCED = new GameProfile(
            /*cpuMin*/ 1200, /*cpuMax*/ 2800, /*gpu*/ 80,
            /*touch*/ 500, /*thermal*/ 42, /*memLock*/ 2048
        );
    }
    
    public void activateGameMode(Context ctx, GameProfile profile) {
        // 1. Boost CPU to performance mode
        setCpuGovernor("performance");
        setCpuMaxFreq(profile.cpuMaxFreq);
        setCpuMinFreq(profile.cpuMinFreq);
        
        // 2. Lock game in memory using cpuset
        lockProcessInCpuset(android.os.Process.myPid());
        
        // 3. Disable notifications
        disableNotifications(ctx);
        
        // 4. Set touch sample rate
        setTouchSampleRate(profile.touchSampleRate);
        
        // 5. Configure thermal throttle
        setThermalLimit(profile.thermalLimit);
        
        // 6. Enable game mode system property
        SystemProperties.set("sys.game.mode", "1");
        
        // 7. Kill background sync
        killBackgroundSyncServices();
        
        Log.i(TAG, "Game Mode Activated: " + profile);
    }
    
    private void setCpuGovernor(String governor) {
        try {
            // Write to all CPU governors
            File govFile = new File("/sys/devices/system/cpu/cpu0/cpufreq/scaling_governor");
            if (govFile.exists()) {
                FileWriter fw = new FileWriter(govFile);
                fw.write(governor);
                fw.close();
            }
        } catch (IOException e) {
            Log.e(TAG, "Failed to set CPU governor", e);
        }
    }
    
    private void lockProcessInCpuset(int pid) {
        // Use cpuset to pin game process
        String cpusetPath = "/dev/cpuset/foreground/boost";
        try {
            // Create foreground boost cpuset if not exists
            Runtime.getRuntime().exec("mkdir -p " + cpusetPath);
            Runtime.getRuntime().exec("chown system.system " + cpusetPath);
            
            // Move process to foreground boost cpuset
            FileWriter fw = new FileWriter("/dev/cpuset/foreground/boost/tasks");
            fw.write(String.valueOf(pid));
            fw.close();
        } catch (IOException e) {
            Log.e(TAG, "Failed to lock process in cpuset", e);
        }
    }
    
    private void setTouchSampleRate(int hz) {
        // Write to touch driver sysfs
        try {
            File rateFile = new File("/sys/class/touchscreen/panel/sample_rate");
            if (rateFile.exists()) {
                FileWriter fw = new FileWriter(rateFile);
                fw.write(String.valueOf(hz));
                fw.close();
            }
        } catch (IOException e) {
            Log.e(TAG, "Failed to set touch rate", e);
        }
    }
}
```

### 6.3 Touch Optimization (1000Hz Sampling)

```c
// ============================================================================
// TOUCH DRIVER PATCH - drivers/input/touchscreen/novatouch.c
// Patch to enable 1000Hz touch sampling
// ============================================================================

/* Add to driver initialization */
static int novatouch_parse_dt(struct novatouch_data *ts)
{
    // ... existing code ...
    
    /* NOVA OS: Enable high-frequency polling */
    ts->report_rate = 1000;  /* 1000Hz instead of default 60Hz */
    
    /* Configure hardware buffer for faster polling */
    ts->hw_buf_size = 512;   /* Larger buffer for high-frequency */
    
    return 0;
}

/* Modify interrupt handler for lower latency */
static irqreturn_t novatouch_irq_handler(int irq, void *dev_id)
{
    struct novatouch_data *ts = dev_id;
    unsigned long flags;
    
    /* NOVA OS: Spinlock for minimum latency */
    raw_spin_lock_irqsave(&ts->lock, flags);
    
    /* Read touch data immediately */
    novatouch_read_data(ts);
    
    /* Process and report with minimal delay */
    input_mt_sync_frame(ts->input_dev);
    input_sync(ts->input_dev);
    
    raw_spin_unlock_irqrestore(&ts->lock, flags);
    
    return IRQ_HANDLED;
}
```

### 6.4 Thermal Management

```java
// ============================================================================
// NOVA THERMAL SERVICE - ThermalProfileManager.java
// ============================================================================

package com.nova.thermal;

public class ThermalProfileManager {
    private static final String TAG = "NovaThermal";
    
    // Thermal throttle thresholds (°C)
    public static class ThermalThresholds {
        public static final int LIGHT_THROTTLE = 40;   // Slight FPS reduction
        public static final int MODERATE_THROTTLE = 43; // 20% CPU reduction
        public static final int HEAVY_THROTTLE = 46;    // 50% CPU reduction
        public static final int CRITICAL = 48;          // Thermal shutdown warning
    }
    
    // FPS target profiles
    public static class FpsProfile {
        public static final int STABLE_60 = 60;
        public static final int ADAPTIVE_45 = 45;       // Drop to 45 for stability
        public static final int ADAPTIVE_30 = 30;       // Emergency fallback
        
        public static int calculateTarget(int temp, int targetFps) {
            if (temp < ThermalThresholds.LIGHT_THROTTLE) {
                return targetFps;  // Full FPS
            } else if (temp < ThermalThresholds.MODERATE_THROTTLE) {
                return Math.min(targetFps, STABLE_60);
            } else if (temp < ThermalThresholds.HEAVY_THROTTLE) {
                return ADAPTIVE_45;
            } else {
                return ADAPTIVE_30;
            }
        }
    }
    
    public void applyThermalProfile(Context ctx, int currentTemp) {
        if (currentTemp < ThermalThresholds.LIGHT_THROTTLE) {
            // No throttling needed
            setCpuThrottle(100);
            setGpuThrottle(100);
        } else if (currentTemp < ThermalThresholds.MODERATE_THROTTLE) {
            // Light throttle - reduce background cores
            setCpuOnlineCores(4);  // Only use 4 cores
            setCpuThrottle(90);
        } else if (currentTemp < ThermalThresholds.HEAVY_THROTTLE) {
            // Moderate throttle - aggressive
            setCpuOnlineCores(2);
            setCpuThrottle(70);
            setGpuThrottle(80);
        } else {
            // Critical - minimal performance
            setCpuOnlineCores(2);
            setCpuThrottle(50);
            setGpuThrottle(60);
            showThermalWarning(ctx);
        }
    }
}
```

---

## 7. Content Creator Features Architecture

### 7.1 Camera2 API Full Support

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CAMERA2 API LEVEL 3 SUPPORT                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Implementation in: frameworks/av/camera/Camera2Client.cpp        │
│                                                                      │
│   ┌────────────────────────────────────────────────────────────┐   │
│   │                  REQUIRED FEATURES                          │   │
│   │                                                            │   │
│   │  ✓ android.sensor.info.rollingSkew = 0                    │   │
│   │  ✓ android.sensor.info.maxAnalogSensitivity = 3200        │   │
│   │  ✓ android.request.maxNumOutputStreams = 5               │   │
│   │  ✓ android.request.maxNumInputStreams = 1               │   │
│   │  ✓ android.request.maxNumReprocessStreams = 4            │   │
│   │  ✓ android.request.availableToneMapModes = [1,2,3,4,5]   │   │
│   │  ✓ android.request.availableFaceDetectModes = FULL       │   │
│   │  ✓ android.scaler.availableStreamConfigurations = FULL   │   │
│   │  ✓ android.scaler.availableMinFrameDurations = FULL     │   │
│   │  ✓ android.scaler.availableStallDurations = FULL        │   │
│   │                                                            │   │
│   └────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Device HAL Requirements:                                           │
│   • API Version: 3.5 (ANDROID_HAL_API_VERSION_1_5)                  │
│   • Capabilities: LOGICAL_MULTI_CAMERA, RAW, BURST_CAPTURE        │
│   • Reprocessing: GPU post-processing support                      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 7.2 Screen Recorder (Creator Studio)

```java
// ============================================================================
// NOVA SCREEN RECORDER - ScreenRecordService.java
// Lightweight, high-quality screen recorder with internal audio
// ============================================================================

package com.nova.creator;

import android.media.MediaCodec;
import android.media.MediaFormat;
import android.media.MediaMuxer;
import android.media.projection.MediaProjection;
import android.os.Handler;
import android.os.HandlerThread;

public class NovaScreenRecorder {
    
    public static class RecorderConfig {
        public static final int VIDEO_BITRATE_4K = 50_000_000;    // 50 Mbps
        public static final int VIDEO_BITRATE_1080P_60 = 25_000_000; // 25 Mbps
        public static final int VIDEO_BITRATE_1080P_30 = 15_000_000; // 15 Mbps
        public static final int AUDIO_BITRATE = 320_000;          // 320 kbps
        
        public int width = 1920;
        public int height = 1080;
        public int frameRate = 60;
        public int videoBitrate = VIDEO_BITRATE_1080P_60;
        public int audioBitrate = AUDIO_BITRATE;
        public boolean recordAudio = true;
        public boolean recordInternalAudio = true;  // Audio bypass for gaming
        public String outputPath;
    }
    
    private MediaCodec createVideoEncoder(RecorderConfig config) {
        MediaFormat format = MediaFormat.createVideoFormat(
            MediaFormat.MIMETYPE_VIDEO_AVC,
            config.width,
            config.height
        );
        
        // High profile for quality
        format.setInteger(MediaFormat.KEY_COLOR_FORMAT, 
            MediaCodecInfo.CodecCapabilities.COLOR_FormatSurface);
        format.setInteger(MediaFormat.KEY_BIT_RATE, config.videoBitrate);
        format.setInteger(MediaFormat.KEY_FRAME_RATE, config.frameRate);
        format.setInteger(MediaFormat.KEY_I_FRAME_INTERVAL, 1);
        
        // KEY_PROFILE_LEVEL for HEVC support
        format.setInteger(MediaFormat.KEY_PROFILE, 
            MediaCodecInfo.CodecProfileLevel.AVCProfileHigh);
        format.setInteger(MediaFormat.KEY_LEVEL, 
            MediaCodecInfo.CodecProfileLevel.AVCLevel41);
        
        return MediaCodec.createEncoderByType(MediaFormat.MIMETYPE_VIDEO_AVC);
    }
    
    // Internal audio capture (no headphone required)
    private AudioRecord createInternalAudioCapture() {
        int sampleRate = 48000;
        int channelConfig = AudioFormat.CHANNEL_IN_MONO;
        int audioFormat = AudioFormat.ENCODING_PCM_16BIT;
        
        // Use AudioPlaybackCapture for internal audio (Android 10+)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
            return createPlaybackCapture();
        }
        
        // Fallback to AAudio for lowest latency
        return createAAudioCapture();
    }
}
```

### 7.3 I/O Scheduler Optimization

```bash
# ============================================================================
# I/O SCHEDULER CONFIGURATION
# For fast video rendering and file transfers
# ============================================================================

# ============================================================================
# BLOCK LAYER TUNING - /sys/block/*/queue/scheduler
# ============================================================================

# Preferred schedulers by use case:
# 
# FOR GAMING:
#   mq-deadline - Lowest latency for single process I/O
#   kyber - Better for mixed workloads
#
# FOR CONTENT CREATION (Video editing):
#   bfq - Best for sequential multi-process I/O
#   mq-deadline - Best for random access (rendering)

# Set scheduler for eMMC/UFS storage
echo "mq-deadline" > /sys/block/sda/queue/scheduler

# Set scheduler for external SD card
echo "bfq" > /sys/block/sdb/queue/scheduler

# ============================================================================
# QUEUE DEPTH OPTIMIZATION
# ============================================================================

# Increase queue depth for better parallelism
echo 64 > /sys/block/sda/queue/nr_requests

# Read-ahead optimization (reduce for NVMe, increase for eMMC)
echo 128 > /sys/block/sda/queue/read_ahead_kb

# ============================================================================
# KERNEL TUNING - /etc/sysctl.d/99-nova-io.conf
# ============================================================================

# Increase max readahead
vm.max_readahead_kb = 512

# Reduce fsync delay (improves I/O throughput, slight data risk)
fsync = 0  # For gaming only - disable for safety

# Increase block layer throughput
blk_iopoll = 50
blk_maxreq = 32
```

---

## 8. User Interface Design

### 8.1 Nova Launcher Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                       NOVA LAUNCHER DESIGN                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Design Principles:                                                 │
│   • Zero animations (or optional minimal)                           │
│   • Instant response (< 50ms touch-to-action)                       │
│   • Minimal memory footprint (< 50MB RAM)                          │
│   • Deep customization without performance cost                    │
│                                                                      │
│   ┌──────────────────────────────────────────────────────────┐      │
│   │                    HOME SCREEN                            │      │
│   │  ┌────┐ ┌────┐ ┌────┐ ┌────┐                             │      │
│   │  │App1│ │App2│ │App3│ │App4│                             │      │
│   │  └────┘ └────┘ └────┘ └────┘                             │      │
│   │  ┌────┐ ┌────┐ ┌────┐ ┌────┐                             │      │
│   │  │App5│ │App6│ │App7│ │App8│                             │      │
│   │  └────┘ └────┘ └────┘ └────┘                             │      │
│   │                                                           │      │
│   │  ─────────────────────────────────────────────           │      │
│   │  [Nova Bar - Quick Actions]                              │      │
│   │  [⚡ Game] [🎮 Record] [📷 GCam] [⚙ Settings]            │      │
│   └──────────────────────────────────────────────────────────┘      │
│                                                                      │
│   Key Features:                                                      │
│   • Single-layer app drawer (swipe up)                              │
│   • Dock with 4 quick action buttons                                │
│   • Minimal widget support (clock, weather only)                   │
│   • Dark theme default (OLED black for battery)                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 8.2 Settings App - Nova Control Center

```
┌─────────────────────────────────────────────────────────────────────┐
│                    NOVA CONTROL CENTER                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌────────────────────────────────────────────────────────────┐     │
│   │                    NOVA CONTROL CENTER                     │     │
│   │   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │     │
│   │                                                            │     │
│   │   PERFORMANCE                                             │     │
│   │   ┌─────────────────────────────────────────────────────┐ │     │
│   │   │ [⚡] CPU Governor      [Schedutil ▾]                │ │     │
│   │   │ [🔥] GPU Perf Mode     [Performance ▾]              │ │     │
│   │   │ [💾] RAM Optimization [Aggressive ▾]                │ │     │
│   │   │ [🔲] Display Refresh   [120Hz ▾]                    │ │     │
│   │   └─────────────────────────────────────────────────────┘ │     │
│   │                                                            │     │
│   │   GAME MODE                                                │     │
│   │   ┌─────────────────────────────────────────────────────┐ │     │
│   │   │ [🎮] Game Space           [ON/OFF]                  │ │     │
│   │   │ [📵] DND in Game          [ON/OFF]                  │ │     │
│   │   │ [🖐] Touch Boost          [1000Hz ▾]                │ │     │
│   │   │ [🌡️] Thermal Limit       [45°C ▾]                   │ │     │
│   │   └─────────────────────────────────────────────────────┘ │     │
│   │                                                            │     │
│   │   CREATOR                                                  │     │
│   │   ┌─────────────────────────────────────────────────────┐ │     │
│   │   │ [📷] Camera2 API         [Level 3]                   │ │     │
│   │   │ [🎬] Screen Recorder     [Quick Access]             │ │     │
│   │   │ [💾] I/O Scheduler        [mq-deadline ▾]            │ │     │
│   │   └─────────────────────────────────────────────────────┘ │     │
│   │                                                            │     │
│   │   SYSTEM                                                   │     │
│   │   ┌─────────────────────────────────────────────────────┐ │     │
│   │   │ [🗑️] Debloat Manager      [Manage ▸]                 │ │     │
│   │   │ [📦] App Manager          [View All ▸]               │ │     │
│   │   │ [🔄] OTA Updates          [Check Now]                │ │     │
│   │   └─────────────────────────────────────────────────────┘ │     │
│   │                                                            │     │
│   └────────────────────────────────────────────────────────────┘     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 9. Build System & Compilation Guide

### 9.1 Build Environment Setup

```bash
#!/bin/bash
# ============================================================================
# NOVA OS BUILD ENVIRONMENT SETUP
# ============================================================================

# Install required packages (Ubuntu 22.04+)
sudo apt update && sudo apt install -y \
    bc \
    bison \
    build-essential \
    ccache \
    curl \
    flex \
    g++-multilib \
    gcc-multilib \
    git \
    gnupg \
    gperf \
    imagemagick \
    lib32ncurses5-dev \
    lib32readline-dev \
    lib32z1-dev \
    libelf-dev \
    liblz4-tool \
    libncurses5 \
    libncurses5-dev \
    libsdl1.2-dev \
    libssl-dev \
    libxml2 \
    libxml2-utils \
    lzop \
    pngcrush \
    rsync \
    schedtool \
    squashfs-tools \
    xsltproc \
    xz-utils \
    zip \
    zlib1g-dev

# Install OpenJDK 17
sudo apt install -y openjdk-17-jdk

# Configure Java environment
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

# Install Repo tool
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
export PATH=~/bin:$PATH

# Configure Git
git config --global user.name "NOVA Developer"
git config --global user.email "dev@nova-os.io"

# Create build directory
mkdir -p ~/novabuild
cd ~/novabuild
```

### 9.2 Source Code Sync

```bash
#!/bin/bash
# ============================================================================
# SOURCE CODE SYNCHRONIZATION
# ============================================================================

cd ~/novabuild

# Initialize manifest for LineageOS 21 (Android 14)
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0

# Add NOVA OS specific manifests
mkdir -p .repo/local_manifests

# Create local manifest for device-specific trees
cat > .repo/local_manifests/novadevice.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
    <!-- Device specific repository -->
    <project path="device/<vendor>/<device>"
        name="LineageOS/android_device_<vendor>_<device>"
        remote="github"
        revision="lineage-21.0" />
    
    <!-- Vendor blobs (if available) -->
    <project path="vendor/<vendor>"
        name="TheMuppets/proprietary_vendor_<vendor>"
        remote="github"
        revision="lineage-21.0" />
        
    <!-- NOVA OS specific packages -->
    <project path="packages/apps/NovaLauncher"
        name="nova-os/android_packages_apps_NovaLauncher"
        remote="github"
        revision="main" />
        
    <project path="packages/apps/NovaGameSpace"
        name="nova-os/android_packages_apps_NovaGameSpace"
        remote="github"
        revision="main" />
        
    <project path="packages/apps/NovaCreatorStudio"
        name="nova-os/android_packages_apps_NovaCreatorStudio"
        remote="github"
        revision="main" />
        
    <project path="packages/apps/NovaSettings"
        name="nova-os/android_packages_apps_NovaSettings"
        remote="github"
        revision="main" />
</manifest>
EOF

# Sync repositories
repo sync -c -j$(nproc) --force-sync --no-clone-bundle --no-tags
```

### 9.3 Device Configuration

```bash
#!/bin/bash
# ============================================================================
# DEVICE-SPECIFIC CONFIGURATION
# Replace <vendor> and <device> with your actual device identifiers
# ============================================================================

cd ~/novabuild

# Source build environment
source build/envsetup.sh

# Select device
export NOVA_BUILD_DEVICE=<device_codename>  # e.g., "cheetah" for Pixel 7 Pro

# Select target
export TARGET_BUILD_VARIANT=userdebug

# Set kernel configuration
export NOVA_KERNEL_DEFCONFIG=<device>_defconfig

# Build with optimizations
export USE_NOVA_DEBLOOM=1
export NOVA_AGGRESSIVE_RAM_OPT=1
export NOVA_GAME_BOOST=1

# Start build
breakfast <vendor>-<device>
```

### 9.4 NOVA OS Build Script

```bash
#!/bin/bash
# ============================================================================
# NOVA OS BUILD SCRIPT - Full ROM Compilation
# ============================================================================

set -e  # Exit on error

# Configuration
export NOVA_VERSION="1.0.0"
export NOVA_CODENAME="Aurora"
export BUILD_DATE=$(date +%Y%m%d)
export ANDROID_VERSION="14"
export LINEAGE_VERSION="21.0"

# Build variables
export ALLOW_MISSING_DEPENDENCIES=true
export OFFICIAL_BUILD=false
export TARGET_BUILD_VARIANT=userdebug

# NOVA OS specific flags
export NOVA_DEBLOAT=true
export NOVA_GAME_MODE=true
export NOVA_CREATOR_MODE=true
export NOVA_LIGHTWEIGHT=true

# Source environment
cd ~/novabuild
source build/envsetup.sh

# Setup device
breakfast <vendor>-<device>

# Apply NOVA patches before build
echo "Applying NOVA OS patches..."
bash vendor/nova/scripts/apply_patches.sh

# Start build with ccache
ccache -M 100G  # 100GB cache
export USE_CCACHE=1

# Build!
mka nova  # Custom build target for NOVA OS

# Package output
echo "Packaging NOVA OS..."
pack_nova_rom.sh

# Output location
echo "Build complete!"
echo "Output: out/target/product/<device>/NOVA_${NOVA_VERSION}_${BUILD_DATE}.zip"
```

---

## 10. First Technical Steps - Quick Start Guide

### 10.1 Step-by-Step Beginner Guide

```bash
# ============================================================================
# DAY 1: ENVIRONMENT SETUP
# ============================================================================

# Step 1.1: Install Ubuntu 22.04 LTS (bare minimum install)
# Download: https://releases.ubuntu.com/22.04/

# Step 1.2: Update system
sudo apt update && sudo apt upgrade -y

# Step 1.3: Install all dependencies
bash <(curl -s https://raw.githubusercontent.com/nova-os/build-scripts/main/setup-deps.sh)

# Step 1.4: Verify installation
java -version  # Should show OpenJDK 17
repo --version  # Should show repo 2.x
gcc --version  # Should show 11.x or higher

# ============================================================================
# DAY 2: REPO SYNC
# ============================================================================

# Step 2.1: Initialize build directory
mkdir ~/nova-build && cd ~/nova-build

# Step 2.2: Initialize repo
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0

# Step 2.3: Add device manifest
# (Add your device's local manifest here)

# Step 2.4: Sync (this may take 2-4 hours)
repo sync -c -j$(nproc) --force-sync

# ============================================================================
# DAY 3: FIRST BUILD (Vanilla)
# ============================================================================

# Step 3.1: Source build environment
source build/envsetup.sh

# Step 3.2: Setup device
breakfast <your-device>

# Step 3.3: Build vanilla LineageOS first (verify build system works)
mka bacon

# If successful, you now have a working build environment!

# ============================================================================
# DAY 4-7: NOVA OS MODIFICATIONS
# ============================================================================

# Step 4.1: Clone NOVA OS packages
git clone https://github.com/nova-os/packages_apps_NovaLauncher.git
git clone https://github.com/nova-os/packages_apps_NovaGameSpace.git
# ... etc

# Step 4.2: Apply debloat patches
bash vendor/nova/patches/debloat.sh

# Step 4.3: Integrate custom services
# (Detailed in Section 6 & 7 above)

# Step 4.4: Build with NOVA modifications
mka nova-bacon

# Step 4.5: Flash and test
adb sideload nova_*.zip
```

---

## 11. Technical Specifications Summary

### 11.1 System Requirements

| Component | Specification | Notes |
|-----------|--------------|-------|
| **CPU** | 64-bit ARM (AArch64) | Snapdragon 700+ recommended |
| **RAM** | 4GB minimum, 8GB recommended | Target <2GB idle usage |
| **Storage** | 32GB minimum | ~8GB for ROM + apps + data |
| **Kernel** | 5.10+ | Requires kernel source |
| **Bootloader** | Unlocked | Required for custom ROM |

### 11.2 Performance Targets

| Metric | Target | Method |
|--------|--------|--------|
| **Boot Time** | < 15 seconds | To fully interactive |
| **RAM Usage (idle)** | < 1.8GB | No apps, screen on |
| **RAM Usage (gaming)** | < 2.5GB | Game + 2 background apps |
| **Touch Latency** | < 20ms | 1000Hz sampling enabled |
| **Frame Time (gaming)** | 16.67ms | 60 FPS stable |
| **I/O Speed** | > 300 MB/s | Sequential read |
| **App Launch** | < 300ms | Cold start, common apps |

### 11.3 ROM Size Optimization

| Component | Original Size | NOVA Size | Savings |
|------------|--------------|-----------|---------|
| System App | ~800 MB | ~150 MB | 81% |
| Framework | ~400 MB | ~380 MB | 5% |
| Fonts | ~150 MB | ~30 MB | 80% |
| Dictionaries | ~100 MB | ~10 MB | 90% |
| **Total** | ~2.5 GB | ~800 MB | **68%** |

---

## 12. Appendix: Kernel Patches Required

### 12.1 Essential Kernel Changes

```diff
diff --git a/kernel/sched/cpufreq_schedutil.c b/kernel/sched/cpufreq_schedutil.c
--- a/kernel/sched/cpufreq_schedutil.c
+++ b/kernel/sched/cpufreq_schedutil.c
@@ -45,6 +45,14 @@
 
 #define MIN_FREQ_REQ_REJECTED_DURATION_US  (10 * USEC_PER_MSEC)
 
+/* NOVA OS: Gaming boost parameters */
+#define NOVA_BOOST_FREQ_MAX 100
+#define NOVA_BOOST_FREQ_HISPEED 90
+#define NOVA_BOOST_FREQ_THRESH 75
+
 static bool schedutil_shouldReject_freq_request(u64 ca_last_update,
                                                 unsigned int d, bool is_migration)
 {
```

### 12.2 ZRAM Enhancements

```c
// drivers/block/zram/zcomp.c - Add LZ4HC support
static struct zcomp_backend backend_lz4hc = {
    .name = "lz4hc",
    .compress = lz4hc_compress,
    .decompress = lz4hc_decompress,
    .destroy = lz4hc_destroy,
};
```

---

## 13. Future Roadmap

### Phase 1 (v1.0) - Core Foundation
- [x] Architecture blueprint
- [x] Debloat specification
- [x] Memory optimization design
- [ ] Base ROM build
- [ ] Core NOVA apps

### Phase 2 (v1.5) - Gaming Features
- [ ] Game Space implementation
- [ ] Touch optimization
- [ ] Thermal management
- [ ] Performance profiles

### Phase 3 (v2.0) - Creator Suite
- [ ] Camera2 API optimization
- [ ] Screen recorder
- [ ] I/O optimizations
- [ ] Media codec tuning

---

## 14. Conclusion

Blueprint ini menyediakan fondasi lengkap untuk membangun **NOVA OS** - custom ROM ultra-ringan yang dioptimalkan untuk gaming dan content creation. Dengan mengikuti arsitektur dan spesifikasi yang telah diuraikan, pengembang dapat menciptakan ROM yang:

- **Hemat RAM** (< 2GB saat idle)
- **Responsif tinggi** (1000Hz touch, <20ms latency)
- **Stabil dalam gaming** (thermal management, FPS lock)
- **Powerful untuk creator** (Camera2 L3, 60fps recording)
- **Minimalis namun lengkap** (zero bloat, semua fitur esensial ada)

**Next Steps:**
1. Pilih device target dan compile vanilla LineageOS
2. Terapkan debloat script
3. Implementasi Game Space
4. Integrasi Creator Studio
5. Test dan optimize berdasarkan hardware spesifik

---

*Document Version: 1.0*
*Last Updated: 2026-07-14*
*Author: NOVA OS Architecture Team*
