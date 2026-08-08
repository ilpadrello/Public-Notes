It is possible to run devtools (in chrome) for the phone.
You will be able to see the phone screen and see the devtools but In order to do so you need : 

- ADB drivers installed in your windows machine (they are brand dependent so find the one's for you brand)
- Connect the Android over USB via File transfer cable, not charging
- Developer options must be enabled
- USB debugging (In Developer Option) activated
- You need to be prompt the message : **“Allow USB debugging?”** → check _Always allow from this computer_ → **OK**

If all those steps are done correctly you should be able to see the device doing :

```bash
adb devices

R3CN800DT8W    device
```

If you don't see `device` but `unauthorized` that means that enven if the phone is connected, you didn't allow USB debugging or you didn't check _Always allow from this computer_
If that is the case:
- disconnect the phone (USB)
- in the developer options _Revoke USB debugging authorizations_
- run `adb kill-server`
- remove the `"%USERPROFILE%\.android\adbkey*"` folder
- `adb start-server`
- Connect your phone and make sure you checked everything correctly

### Using the feature
If everything goes as it should, you can open a chrome tab in android, and in your chrome (PC) go to the page :
```
chrome://inspect/#devices
```

Then you should see the list of connected device, with the list of page opened.
you can now inspect and use dev-tools as you would for your desktop version.

### TO REMEMBER
Authorization for PC's have a TTL if not used of about one week, so you will need to re-check that you grant access for that PC a week later.
This behavior can be changed in the developer options.

### OVER WIFI 
For some samsung phones (My phone can do that), it is possible to disconnect the phone and actually do the same thing via WIFI, you don't need to have your phone connected to the PC via USB all time, just be in the same network.


