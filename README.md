# Custom ROM Building Guide using Crave.io

## Introduction

This guide provides a step-by-step approach to building a custom ROM in Crave.io, as outlined by an experienced ROM builder. Follow these instructions carefully to successfully build and upload your ROM.


---

## Step 1: Clone the ROM Base

Crave.io provides multiple ROM bases to choose from. To view the available options, use the following command:

```
crave clone list
```

Once you have identified the ROM base you want to use, create a new project with:

```
crave clone create --projectID 72 <name>
```

Replace <name> with any name of your choice.

The 72 refers to the ROM base ID (e.g., 72 corresponds to LineageOS 21).

You can find other available base IDs using the crave clone list command.


Navigate into the project directory:

```
cd <name>
```

Establish an SSH session with the build environment:

```
crave ssh
```


---

## Step 2: Clone the Local Manifest

To efficiently clone the device tree (dt), vendor, and kernel, use:

```
git clone http://name/local_manifest_chime.git -b A14 .repo/local_manifests
```

This approach prevents manual cloning of each component separately.


---

## Step 3: Initialize the ROM Source

Visit the ROM source repository on GitHub and initialize it with the following command:

```
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs
```


---

## Step 4: Synchronize the Source and adaptation

Use the Crave.io synchronization script to sync both the ROM source and the local manifest:

```
/opt/crave/resync.sh
```

Synchronization time varies based on the ROM source size.

After synchronization, the device tree must be adapted to match the ROM. This involves renaming files, such as changing `lineage_<codename>.mk` to `customrom_<codename>.mk` for any needed custom ROM, and replacing `lineage` references with the ROM's name in configuration files. You can check the device tree of any officially supported device in the custom ROM's GitHub repository to see the necessary changes, such as modifying `Device.mk`, `Customrom_<codename>.mk`, and other files. Use these references as guidance to properly adapt your device tree.


---

## Step 5: Test the Build Environment

Since Crave.io does not fully support ROM compilation due to hardware limitations, use the following command in the SSH session to verify the setup:

```
source build/envsetup.sh
brunch chime
```

The brunch command varies depending on the ROM type.

If everything is working correctly, proceed to the next step.


Exit the SSH session:

```
exit
```


---

## Step 6: Queue the Build

To queue the actual ROM build, run:

```
crave run --no-patch -- "
source build/envsetup.sh
brunch chime
"
```

Wait for the build process to complete.


---

## Step 7: Retrieve the Built ROM

Once the build succeeds, locate the generated ROM file. Navigate to the log and scroll down to find a path similar to:

```
out/target/product/blossom/DerpFest-15-Community-Stable-blossom-20241225.zip
```

Use the following command to pull the built ROM from the build server:

```
crave pull out/target/product/blossom/Lineage-15-Community-Stable-chime-20241225.zip
```

---

## Step 8: Upload the ROM

To upload the built ROM, you can use Telegram or PixelDrain.

### Upload via Telegram (2GB max limit):

```
telegram-upload out/target/product/blossom/Lineage-15-Community-Stable-chime-20241225.zip
```

### Upload via PixelDrain:

```
curl -T "out/target/product/*/" -u :<api> https://pixeldrain.com/api/file/
```


---

## Conclusion

Following these steps will help you efficiently build and distribute your custom ROM using Crave.io. If any issues arise, refer to the logs for debugging.

Happy ROM Building!

