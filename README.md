# Blizzard Device Identification 
The main purpose of this repository is to explain the internal Overwatch functions of the Blizzard-Device-Identification system.

The reason we decided to create this repository was because some people (*which we wont name, you know who you are*) are claiming they know what is going on, without actually ever dumping or reversing the game. (their words lol) It is true that the majority of logic is hidden behind servers, so nobody knows for certain what is happening, but what we do know is that Blizzard's device identification system isn't some dark magic. The system combines multiple sources, such as ACP or Computer Name to generate the Device-ID, which will then be send with each HTTP-Request. (*You can see this in the Dump, if you know how to do it.*) 

The Device-ID is saved in the Registry Path: `HKEY_CURRENT_USER\Software\Blizzard Entertainment\Battle.net\Identity`. 

Data that is used to generate this Device-ID is as follows:
- [ACP](https://docs.microsoft.com/en-us/windows/win32/api/winnls/nf-winnls-getacp)
- [User Default Language ID](https://docs.microsoft.com/en-us/windows/win32/api/winnls/nf-winnls-getuserdefaultlangid)
- [System Default Language ID](https://docs.microsoft.com/en-us/windows/win32/api/winnls/nf-winnls-getsystemdefaultlangid)
- [Computer Name](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-getcomputernamew) (*Anti-Flag changes this*)
- [Username](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-getusernamew)
- [Timezone Information](https://docs.microsoft.com/en-us/windows/win32/api/timezoneapi/nf-timezoneapi-gettimezoneinformation)
- [CPU ID](https://docs.microsoft.com/en-us/cpp/intrinsics/cpuid-cpuidex?view=msvc-160)
- [RTL Version](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-rtlgetversion)

If you don't want Blizzard to be able to identify you, you can simply delete the device id from the registry path, rename your PC and/or use [Anti-Flag](https://github.com/dword64/Ow-Anti-Flag).

From testing we can also confirm the login-whitelist for each account isn't device-id based but either IP-based or Mac-Based. (We don't know this, because this is serverside stuff)

If you switch VPN you will have to verify yourself again though, if you log in that is. (Captcha etc.) 

Some people are also claiming that Blizzard's "Your Account has been locked due to suspicious activity" screen means anything. But it doesn't, it just means your IP/MAC isnt verified/whitelisted for this account. After logging in via Battle.Net, your IP/Mac/Whatever will be verified and you will be fine logging into the account. If you combine this with Anti-Flag you will seem like a completely new device. 

If you look at the right spots you can even see this Device-ID in some log files.

There were also some rumors going around that Battle.net "catches you", but after reversing that aswell we weren't even able to find the device-id function from Overwatch. 
Battle.net isn't even obfuscated, encrypted or has an anti-debug. So if you want you can even look yourself :) 

There are some more tricks to this dillema but we won't share the secret sauce. 

## Dump Screenshots
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/check_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/get_sys_info_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/validate_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/core_device_hash_func.png)

## Credits
@AvalonGroup for Reversing it.
