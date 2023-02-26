This is my first VOLATILITY challenge xD<br />
The challege give us a memory dump with the hint volatility. So i guess lets install the tool and use it.<br />
After downloading the tool, lets start running some coomand on this. Since this is my first time using volatility, lots of googling was used xD.<br />
First command that i ran was:<br />
```python2 volatility/vol.py -f memory.raw imageinfo```<br />

Using this command we can get the info about the dump, especially the profile of it<br />
![image](https://user-images.githubusercontent.com/109911533/221234887-96b0a42d-df50-4388-b818-e3ddfd8b31d6.png)<br />

After having the profile, which is WinXPSP2x86, WinXPSP3x86(you can choose one of them, i guess), now lets get inhto some business.<br />

First, the chall said something about executing something malicious, so after some google, seem like we have to use the "consoles" command, which then gives us the third part of the flag.<br />
![image](https://user-images.githubusercontent.com/109911533/221235800-7c9bbf64-ee27-4610-a174-d31900a2314e.png)<br />

Secondly, lets run filescan to see all the files from the memory dump<br />
![image](https://user-images.githubusercontent.com/109911533/221416301-f054582d-63fe-4374-bd9c-225b521d9d3e.png)<br />

Looking at the output, you will find 2 interesting files which is wallpaper.bmp and secret_2.zip. Lets dump out those files.<br />
![image](https://user-images.githubusercontent.com/109911533/221416428-0c36c0ef-bf7c-4501-a0c3-2a06bfb9150c.png)<br />

After dumping them out, the secret_2.zip has a .txt file in it, which need a password. Then, if you open the wallpaper.bmp file, you'll see that the password is "found within you".<br />
![image](https://user-images.githubusercontent.com/109911533/221416535-8bed59c9-491b-401b-b18a-73c8004ef845.png)<br />

Entering the password to the zip file gives us the second part of the flag.<br />
![image](https://user-images.githubusercontent.com/109911533/221416643-32095c24-4ac7-42f4-af6d-ac7ef6bc1d90.png)<br />

Now, for the first part. This took me most of the time since i dont know what command to run (first time with volatility), so after some hours, i decided to ask for help from the admin, which he hinted me that the first part is encoded and the command for it is among the most common one. So i guess i'll have to run some command and look again.<br />

After some tries, the command that we need to use is envars, which return us secret 1.<br />
![image](https://user-images.githubusercontent.com/109911533/221417315-24b00bb5-416c-4a72-b7f6-88d7a3d38194.png)<br />

Decode it with base64 and we got the full flag.

___Flag: Securinets{1f_h3_l0v3s_y0u,_tha7's_th3_m05t_d4ng3rous_th1ng._YOU,_Netflix!} ___





