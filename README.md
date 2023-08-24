**中文:**
找遍全网只找到一个kernelsu安卓13版本的,安卓13frida兼容不好,只能自己编译一个安卓12版本的,修改了反调试检查点,随缘更新。

注意不要登录谷歌账号,否则GooglePlay系统自动更新到最新frida一样会有兼容问题。

我还提供了我编译的frida-server,配合我的frida-server以及下面这个frida脚本可以过大部分检查。

**English:**

I searched the entire web and only found a version of kernelsu for Android 13. Android 13 is not very compatible with Frida, so I had to compile a version for Android 12 myself, modifying the anti-debugging checkpoints, and updating randomly as the situation dictates.

Be careful not to log in to your Google account, or the Google Play system will automatically update to the latest Frida, and compatibility issues may arise.

I have also provided the frida-server I compiled. Along with my frida-server and the following Frida script, most checks can be bypassed.

**Frida Script:**
```javascript
//PrettyMethod check
function fridaInlineHookCheckPass() {
    //get libart module 
    var libart = Process.findModuleByName("libart.so");
    console.log("libart base: " + libart.base);

    //get you cellphone libart.so use idapro find PrettyMethod function offset 
    var PrettyMethodOffset = 0x175474;
    var PrettyMethod = libart.base.add(PrettyMethodOffset);

    //check old function head code
    console.log(myhexdump(Memory.readByteArray(PrettyMethod, 0x20)));

    Memory.protect(PrettyMethod, 16, "rwx");

    var p_fun = new NativePointer(PrettyMethod);
    p_fun.writeByteArray([0xF0, 0xB5, 0x85, 0xBD, 0x04, 0x46, 0x3E, 0x48, 0x0D, 0x46]);

    Memory.protect(PrettyMethod, 16, "r-x");

    //check new function head code
    console.log(myhexdump(Memory.readByteArray(PrettyMethod, 0x20)));
}


Android version:12 
Download Google ROM: https://developers.google.com/android/images?hl=zh-cn#coral
Android Build number:sp1a.210812.015
adb reboot bootloader
fastboot flash boot boot.img 
