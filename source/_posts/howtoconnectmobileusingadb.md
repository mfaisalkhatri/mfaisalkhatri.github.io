---
title: How to connect mobile to PC using ADB
date: 2020-05-30 11:38:20
tags: [Test Automation, QA, Tutorial]
---

{%asset_img header_image.jpg Header Image%}

Hello,
    In this Post I will be discussing about how to connect your mobile phone to PC using ADB command.
    Lets go step by step to implement this.

- **STEP ONE**: 

    Install Android Studio on your PC.

- **STEP TWO**: 
    
    Create "ANDROID_HOME" environment variable by giving android/sdk path in it.

- **STEP THREE**: 
    **Enable "USB Debugging" in your phone.**

    01. Go to your Phone Settings >> About >> Tap the Build Number/MIUI version 7 times to enable "Developer Options".

    {%asset_img Settings_Version.png Phone Settings%}

    02. In Developer Options >> Enable USB Debugging.

    03. In Developer Options >> Enable USB Debugging(Security Settings) <In case of MI Phones>.

    {%asset_img Developer_Options_USB_Debuggin_On.png Enable USB Debugging%}

- **STEP FOUR**: 
    **Connect your phone to your PC using USB.**
  01. Run the command "adb devices". It will give output as follows: 
      `List of devices attached  <encrypted device id> device`

        {%asset_img Adb_Devices_1.png Enable ADB devices%}

  02. Run the command `adb tcpip 5555`, it will restart adb on default port.

        {%asset_img Adb_tcpip_connect.png tcpip connect%}

  03. Go to your Phone Settings >> Status >> Check Ip of your phone.
      
        {%asset_img Settings_Status_MobileIP.png Mobile IP%}

  04. Run the command in command prompt of your pc: `adb connect <ip address of your phone> <Press Enter>`
      If the everything is successful, command prompt will displayed message as 
      `connected to <ip address of your phone>`.

  05. Again run the command `adb devices`, it will show 2 devices as follows:
      List of devices attached 
      `<encrypted device id>   device`
      `<ip of your phone>      device`
      {%asset_img Adb_connect_devices.png ADB Devices%}

  06. Now, disconnect your phone from pc and again run the command `adb devices`, it will show only 1 device connected as follows:
      List of devices attached `<ip of your phone> device`

- Congratulations, you have successfully connected your device to your PC, now you don't need to keep your device connected through USB all the time.