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

I tried this https://www.frida.re/docs/installation/, it doesn't work on mac.

https://www.frida.re/docs/modes/. Frida basically support three different modes:
Injected, Embedded, Preloaded

# List of tools in frida:
`frida-ps`: listing processes

`frida-trace`: dynamically tracing function calls
`-i/-x` include/exclude function, `-I/-X` include/exclude module, `-f` spawn FILE.
`module/function/object`.

`frida-discover`: discover internal functions in a program, which can be traced by using frida-trace

`frida-ls-devices`: listing attached devices

`frida-kill`: command-line tool for killing processes.

# Frida python APIs
How does it work?

# My tries
https://www.freebuf.com/articles/system/190565.html
The first experiment, to change the argument of function:
Some findings:
 - It's using javascript to modify the java function, it's interesting to see the
 capability of javascript on java code.
 - The output is re-directed to python (host).
 - When running the python code, there will be a new process id generated, but not  when  python stop.
 (The new process is generated when spawn)

 Function overload

 Remote execution

# Notice
Remember to use `-U` option  of the commands to monitor the USB instead of the host machine

# Use MobSF
- Need to use Genymotion(https://www.genymotion.com/fun-zone/, download personal edition, you need to create an account).
- Need to install https://github.com/m9rco/Genymotion_ARM_Translation, seems not support android 9. Tutorial(https://pentester.land/tips-n-tricks/2018/10/19/installing-arm-android-apps-on-genymotion-devices.html)
- Activity Tester, Exported Activity Tester
