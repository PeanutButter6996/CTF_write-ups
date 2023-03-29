# picoCTF FORENSIC #

### Challenge: INVISIBLE WORDs ###

- We are given a bmp (bitmap) file. The first thing comes in my mind when given a bitmap file is HexEdit.<br />

![image](https://user-images.githubusercontent.com/109911533/228514470-1d7bdb3d-e526-4240-b040-d319511440f4.png)<br />

- Open it in HexEdit, we got a lot of things to look at.
  - 1: the ```header```.
  - 2: the ```data``` of that bmp file.

![image](https://user-images.githubusercontent.com/109911533/228515219-b72b43ea-498c-412d-9638-512bae0c9b29.png)<br />

- Now before we begin, there are 2 things you should do.
  - Go on Wikipedia and try to understand about bitmap file format. This is the link ```https://en.m.wikipedia.org/wiki/BMP_file_format```
  - Now if you know or have done some forensic challenge before, you should notic a ```PK header```, which indicates a ```ZIP file```. If you notice that, then that should kind of gives you the general way to solve this challenge, which is to retrieve the hidden ```ZIP file```.

- Now for this chalenge, the hint is kind of suck and stupid to be honest, tooks me a really long time just to understand. Reading through the documentation about bmp file on Wiki, you should notice what so called a ```bitmask```, which is use to define the pixel format, and they are called ```DWORD```, which this can be the ```Invisible WORD``` that the challenge is talking about.<br />
- Now lets read about ```bitmask``` and see what they can do with the bmp file.

```https://en.wikipedia.org/wiki/Mask_(computing)```
```https://realpython.com/python-bitwise-operators/#bitwise-and```

- Now read through all of that, you can basically understand like this.
  - The bitmap ```bitmask``` hex values start at offset ```36h```, in the order of RGBA, which is ```Red, Green, Blue, Alpha```.
  - Each of the ```bitmask``` will ```XOR, AND, OR,...``` with each of the pixel to return the color of the bitmap file.( in this challenge should be AND)

![image](https://user-images.githubusercontent.com/109911533/228521574-4db6a9d7-f63b-4168-a4f0-aa087d769a91.png)

- Now that we understand that, lets check to see if we are right. Taking the first ```bitmask```, which is ```00 7C 00 00 AND 38 67 50 4B```, the ```00 00 AND 50 4B``` should return as ```00 00```.
- Returing as ```00 00```, then the bmp file will kind of like IGNORE those bytes, which would make them **INVISIBLE**. Mind blown right?? xD
- Now all we have to do is to first extract all the hex values, starting with ```38 67 50 4B``` and end at ```08 2C 00 00``` (because the rest of the file will contain random bytes which is ```FF 00```).
- Then write a script to extract only the last 2 bytes of 4 bytes, it would mean for example ```38 67 50 4B```, we only want ```50 4B```, and do so with every else until we got to the end of all the hex values.
- The end product should look like this.

![image](https://user-images.githubusercontent.com/109911533/228523888-7fbcb2a0-2fa7-4e34-b590-30522ff4b67c.png)<br />

- Now take all those hex values, paste them into Cyber Chef, use the ```From HEX``` function, out put them as a zip file, it should return us with a ZIP file contain a ```.txt``` file which holds the flag.

![image](https://user-images.githubusercontent.com/109911533/228526915-14aeab96-4f23-447f-a28d-f77fd0d2b88a.png)

- Open the file in notepad and search for ```picoCTF``` shoudl return you the flag.

**flag: picoCTF{w0rd_d4wg_y0u_f0und_5h3113ys_m4573rp13c3_539ea4a8}**

