# Blizzard Device Identification 
The main purpose of this repository is to explain the internal Overwatch functions of the Blizzard-Device-Identification system.

The reason we decided to create this repository was because some people (*which we wont name, you know who you are*) are claiming they know what is going on, without actually ever dumping or reversing the game. (their words lol) It is true that the majority of logic is hidden behind servers, so nobody knows for certain what is happening, but what we do know is that Blizzard's device identification system isn't some dark magic.

If you launch either Overwatch or close Battle.net, a new Device-ID (Identity) will be generated. This Device-ID is a randomly generated 64 byte UUID.  

The Device-ID is saved in the Registry Path: `HKEY_CURRENT_USER\Software\Blizzard Entertainment\Battle.net\Identity`. 

If you don't want Blizzard to be able to identify you, you can simply delete the device id from the registry path, rename your PC and/or use [Anti-Flag](https://github.com/dword64/Ow-Anti-Flag).

From testing we can also confirm the login-whitelist for each account isn't device-id based but either IP-based or Mac-Based. (We don't know the details, because this is serverside stuff)

If you switch VPN you will have to verify yourself again though, if you log in that is. (Captcha etc.) 

Some people are also claiming that Blizzard's "Your Account has been locked due to suspicious activity" screen means anything. But it doesn't, it just means your IP/MAC isnt verified/whitelisted for this account. After logging in via Battle.net (You can also do this via the Battle.net website), your IP/Mac/whatever will be verified and you will be fine logging into the account. If you combine this with Anti-Flag you will seem like a completely new device. 

If you look at the right spots you can even see this Device-ID in some log files.

There were also some rumors going around that Battle.net "catches you", which isn't true. Battle.net **will** generate a new device-id (for example if you delete it and then log out of your account), but its the same system as in the client.

There are some more tricks to this dillema but we won't share the secret sauce. 

##
```
00 RGKY = Registry Key              -> SOFTWARE\Blizzard Entertainment\Battle.net\Identity\Identity
01 CPGE = Code Page                 -> GetACP
02 ULNG = User Lang                 -> GetUserDefaultLangID
03 SLNG = System Lang               -> GetSystemDefaultLangID
04 CNME = Computer Name             -> GetComputerNameW
05 UNME = User Name                 -> GetUserNameW
06 UTCO = UTC Offset                -> -GetTimeZoneInformation.Bias
07 CARC = CPU Architecture          -> GetNativeSystemInfo.wProcessorArchitecture
08 CREV = CPU Revision              -> GetNativeSystemInfo.wProcessorRevision
09 CLVL = CPU Level                 -> GetNativeSystemInfo.wProcessorLevel
10 PMEM = Physical Memory           -> GlobalMemoryStatusEx.ullTotalPhys >> 20
11 PSZE = Page Size                 -> GlobalMemoryStatusEx.ullTotalPageFile >> 20
12 OVER = OS Version                -> RtlGetVersion as "dwPlatformId.dwMajorVersion.dwMinorVersion.dwBuildNumber szCSDVersion"
13 CVRA = CPU Version Info          -> __cpuid(1).EAX
14 CFTC = CPU Features ECX          -> __cpuid(1).ECX
15 CFTD = CPU Features EDX          -> __cpuid(1).EDX
16 CEFC = CPU Extended Features ECX -> __cpuid(80000001).ECX & (AMD   ? 0x00000000 : 0xFFFFFFFE)
17 CEFD = CPU Extended Features EDX -> __cpuid(80000001).EDX & (INTEL ? 0xFFFFF7FF : 0x00000000)
18 CBRD = CPU Brand                 -> __cpuid(80000002, 80000003, 80000004).(EAX, EBX, ECX, EDX)
19 CVEN = CPU Vendor                -> __cpuid(0).(EBX, ECX, EDX)
20 ANME = Adapter Name              -> GetAdaptersInfo.AdapterName
21 ADSC = Adapter Description       -> GetAdaptersInfo.Description
22 MAC\ = Adapter MAC Address       -> GetAdaptersInfo.Address
```

## Dump Screenshots
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/check_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/get_sys_info_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/validate_hash.png)
![infos](https://github.com/Avalon-Group/Blizzard-Device-Identification-Explained/blob/main/images/core_device_hash_func.png)

## Credits
@AvalonGroup for Reversing it.
