# Some WEB challenges that i can solve #
##### Uoc gi dc clear web #####

## 1. Catch me if you can ##
- Simple, look at source code<br />

**Flag: actf{y0u_caught_m3!_0101ff9abc2a724814dfd1c85c766afc7fbd88d2cdf747d8d9ddbf12d68ff874}**

## 2. Celeste Speedrunning Association ##
- Acess the ```play``` endpoint to press the button.
- Using BurpSuite, press the button and access the submit endpoint we know why we always lose.

![image](https://user-images.githubusercontent.com/109911533/234898724-3d92a7d0-c0de-41c7-ab9e-abcdf586169d.png)

- The ```start``` variable is set to what looks like the max value of Interger, which my friend ```m3r1t168``` suggests could be ```Interger overflow```. Setting it to ```9999999999``` should do the trick LoL xDD.

**Flag: actf{wait_until_farewell_speedrun}**

## 3. Shortcircuit ##
- Simple crypto-web challenge
- Flag contains 120 characters which is divided to 4 chunks, each contains 30 characters.
- Then they are swapped between each other
- Should be easy since it begins with ```actf{``` and ends with ```}```.

**Flag: actf{cl1ent_s1de_sucks_ffa92516317199e454f4d2bdb04d9e419ccc7544e67ef12024523398ee02fe7517f7e08250c4aaa9ed206fd7c9e398e2}**

## 4.  

