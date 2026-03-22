## Description:
This is a fork from [KonstaT's brilliant work](https://github.com/raspberry-vanilla/android_local_manifest) of Porting AOSP on Raspberry Pi 5.
The main purpose of this fork is to tinker around, explore, learn and experiment about what it take to port AOSP on any custom SoC.
[KonstaT's](https://github.com/KonstaT) work serves as a really great starting point.

### Device specific configuration to build AOSP Android 16 for Raspberry Pi 5.

***

### How to build (Ubuntu 24.04 LTS):

1. Establish [Android build environment](https://source.android.com/docs/setup/start/requirements).

2. Install additional packages:

```
sudo apt-get install dosfstools e2fsprogs fdisk kpartx mtools rsync
```

3. Initialize repo:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-16.0.0_r4
curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/aosp-rpi5/android_local_manifest/android-16.0/manifest_brcm_rpi.xml --create-dirs
```

Or optionally, you can reduce download size by creating a shallow clone and removing unneeded projects:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-16.0.0_r4 --depth=1
curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/aosp-rpi5/android_local_manifest/android-16.0/manifest_brcm_rpi.xml --create-dirs
curl -o .repo/local_manifests/remove_projects.xml -L https://raw.githubusercontent.com/aosp-rpi5/android_local_manifest/android-16.0/remove_projects.xml
```

4. Sync source code:

```
repo sync
```

5. Setup Android build environment:

```
. build/envsetup.sh
```

6. Select the device target (`rpi5`) and build:

```
lunch aosp_rpi5-bp4a-userdebug
```


7. Compile:

```
make bootimage systemimage vendorimage -j$(nproc)
```

8. Make flashable image for the device (`rpi4` or `rpi5`):

```
./rpi5-mkimg.sh
```

### Kernel:

[Linux kernel build instructions](https://github.com/aosp-rpi5/android_kernel_manifest/tree/android-16.0).
