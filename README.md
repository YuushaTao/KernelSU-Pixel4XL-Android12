找遍全网只找到一个kernelsu安卓13版本的,安卓13frida兼容不好,只能自己编译一个安卓12版本的,修改了反调试检查点,随缘更新

注意不要登录谷歌账号,否则GooglePlay系统自动更新到最新frida一样会有兼容问题

Android version:12 

Download Google ROM: https://developers.google.com/android/images?hl=zh-cn#coral

Android Build number:sp1a.210812.015



flash 

adb reboot bootloader

fastboot flash boot boot.img
