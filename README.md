# mob-iot-security
# How to root the device
The device is Google Nexus 4. Android version 5.1.1.
- Connect to a wifi (better a home wifi), I haven't figured out how to connect to CPU
- Enter developer mode
- connect with usb
- Disable apk check from usb
- install Kingo Root
- install adbd insecure (https://blog.csdn.net/hlllmr1314/article/details/52217128)
- **Brida-server crash** (give up)

# How to start emulator

It will be naturally rooted.
Reason for x86 image: it's 10X faster

```
# Install emulator
/usr/local/share/android-sdk/tools/bin/sdkmanager emulator

# Install system image
/usr/local/share/android-sdk/tools/bin/sdkmanager 'system-images;android-28;google_apis;x86'

# Create emulator (name as android9)
/usr/local/share/android-sdk/tools/bin/avdmanager create avd -n android9 -k 'system-images;android-28;google_apis;x86'

# Start emulator
/usr/local/share/android-sdk/emulator/emulator @android9

# How to delete emulator
# rm -rf ~/.android/avd/**file_name**
```

Other useful commands:
```
# list all available system images
sdkmanager --list --verbose

# list system images on your pc
sdkmanager --list
```

# How to setup Frida
Reference
https://www.frida.re/docs/android/
(You may need to download frida-serser 12.7.4)
https://github.com/frida/frida/issues/936
```
# Download frida-server12.7.4
wget -c https://github.com/frida/frida/releases/download/12.7.4/frida-server-12.7.4-android-x86.xz

# Uncompress
brew install xz
xz -d frida-server-12.7.4-android-x86.xz
mv frida-server-12.7.4-android-x86 frida-server

adb root # might be required
adb push frida-server /data/local/tmp/
adb shell "chmod 755 /data/local/tmp/frida-server"
adb shell "/data/local/tmp/frida-server &"

```

Please note the frida-server architecture should be consistent with the emulator
