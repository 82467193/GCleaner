#    gcleaner
**SHA256:0f41bd199f9dced09ce6af8dd974a67322ef3d26722a2d719bec4f2e595cbcf0**

##   Basic analysis
The sample is from **MalwareBazzar**, so we can take some advantages from it cuz the website gives us some resources about its basic information.
![](https://hackmd.io/_uploads/S14lDPz8h.png)

Let us focus on the file type first, it's an EXE file which means it's a Windows platform malware.
And you might be interested in **Inno Setup Installer**, which the author might use because of obfuscation or any other reasons.

![](https://hackmd.io/_uploads/S1UHcDGL3.png)
From the basic information of IP address we infer there might not exist a C&C server, so we are more interested in the dropped files we can see that after the installer has been executed it leaves 5 files three of them are clean, and they might be confusion purpose. And for the rest of two, the one that has a higher detection rate, I strongly suspect it's a trojan, tho we mentioned that this loader might not exist  C&C server, I believe it might exist in the trojan, so so far we have some really basic information about this file.


##    Behavioral analysis
We use Autoruns for searching any persistent related to this loader, I found nothing but I did find some files dropped by the loader after I checked the regshot and procmon.
![](https://hackmd.io/_uploads/rkfwIBX82.png)
![](https://hackmd.io/_uploads/rkFDUS7Ln.png)
Based on the above images, the loader dropped 7 files in total cuz I also count the files dropped by a trojan.
They are
* is-U8T1C.tmp
* _setup64.tmp
* _iscrypt.dll
* _shfoldr.dll
* Rec529.exe
* qEjQRKSEZevdQ.exe
* Preview.exe(Copy)

So far I haven't observed any network communication, but maybe it exists a C&C server like I mentioned before. Now I'm going into the next stage of analysis.

##    Further analysis
Let's see if we can use InnoUnpacker to get some info, I tried the Inno unpacker which I found on the Internet [https://innounp.sourceforge.net/](https://) then I found it doesn't work cuz the author who wrote this malware encrypt the file with the password with salt, so it's nearly impossible to guess the password, tho, in theory, we can find out the password but it will cost us a lotttttttt of time to guess it.

I analyze the Rec529.exe which was created by the dropper directly.

So now based on the above picture I found the malware using the tmp(clean) file to deploy the trojan in this sample are Rec529.exe and qEjQRKSEZevdQ.exe, btw the temp file will be randomly named except Rec529.exe and Preview.exe.

Now our new mission is to focus on what Rec529.exe and qEjQRKSEZevdQ.exe did. Before I go deeper, I execute the Rec529.exe and found the program has accessed URLs and does sth with cmd.exe so I indicate this might be a downloader for further malicious activity.
![](https://hackmd.io/_uploads/SJ6HF6t8h.png)

![](https://hackmd.io/_uploads/Byc6zAtLh.png)

From this sample I found the exe dropped by Rec529.exe but it does nothing, so I guess it might use for further attacks on the C&C server.

#    Conclusion:


Attack Process

![](https://hackmd.io/_uploads/BJRVBPoU3.png)

It first drops and deploys the tmp file and after the main payload is deployed the second stage command and control starts.