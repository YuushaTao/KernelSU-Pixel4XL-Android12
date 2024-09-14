找遍全网只找到一个kernelsu安卓13版本的,安卓13之后frida兼容不好,hook32位的so有问题,只能自己编译一个安卓12版本的,修改了一些反调试检查点等等,随缘更新。

注意不要登录谷歌账号,否则GooglePlay系统会自动更新到最新,frida一样会有兼容问题。

我还提供了我编译的frida-server,配合我的frida-server以及下面这个frida脚本可以过一些检查。


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
```

**System Info:**

Android version:12 

Download Google ROM: https://developers.google.com/android/images?hl=zh-cn#coral

Android Build number:sp1a.210812.015

**USE:**

adb reboot bootloader

fastboot flash boot boot.img 
