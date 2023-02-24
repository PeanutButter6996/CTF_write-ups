This is my first VOLATILITY challenge xD<br />
The challege give us a memory dump with the hint volatility. So i guess lets install the tool and use it.<br />
After downloading the tool, lets start running some coomand on this. Since this is my first time using volatility, lots of googling was used xD.<br />
First command that i ran was:<br />
```python2 volatility/vol.py -f memory.raw imageinfo```<br />

Using this command we can get the info about the dump, especially the profile of it<br />
![image](https://user-images.githubusercontent.com/109911533/221234887-96b0a42d-df50-4388-b818-e3ddfd8b31d6.png)<br />

After having the profile, which is WinXPSP2x86, WinXPSP3x86(you can choose one of them, i guess), now lets get inhto some business.<br />

First, the chall said something about executing something malicious, so after some google, seem like we have to use the "consoles" command, which then gives us the end part of the flag.<br />
![image](https://user-images.githubusercontent.com/109911533/221235800-7c9bbf64-ee27-4610-a174-d31900a2314e.png)
