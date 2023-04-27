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
- Simple crypto-web challenge, solved by my friend ```Giapppp```, you can check out other crypto challenges from different CTF solved by him here ```https://github.com/Giapppp```
- Flag contains 120 characters which is divided to 4 chunks, each contains 30 characters.
- Then they are swapped between each other
- Should be easy since it begins with ```actf{``` and ends with ```}```.

**Flag: actf{cl1ent_s1de_sucks_ffa92516317199e454f4d2bdb04d9e419ccc7544e67ef12024523398ee02fe7517f7e08250c4aaa9ed206fd7c9e398e2}**

## 4. Directory ##
- The challenge gives us a website with 5000 directory, and one of them contains the flag.
- Using Burp Intruder, bruteforcing should take less than 5 minutes.

![image](https://user-images.githubusercontent.com/109911533/234902918-4a85c977-1ec7-490e-8e7f-5657808a0a97.png)

**Flag:actf{y0u_f0und_me_b51d0cde76739fa3}**

## 5. Celeste Tunneling Association ##
- The challnge give us a source, let's take a look
```py
import os

SECRET_SITE = b"flag.local"
FLAG = os.environ['FLAG']

async def app(scope, receive, send):
    assert scope['type'] == 'http'

    headers = scope['headers']

    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            [b'content-type', b'text/plain'],
        ],
    })

    # IDK malformed requests or something
    num_hosts = 0
    for name, value in headers:
        if name == b"host":
            num_hosts += 1

    if num_hosts == 1:
        for name, value in headers:
            if name == b"host" and value == SECRET_SITE:
                await send({
                    'type': 'http.response.body',
                    'body': FLAG.encode(),
                })
                return

    await send({
        'type': 'http.response.body',
        'body': b'Welcome to the _tunnel_. Watch your step!!',
    })
```
- Basically, the server is checking for the host header. If the head exists, then it checks for the value of the head whether it's equal to the secret value or not, then it would return us the flag.
- However looking at the source code closely, you can see that the secret value is ``` SECRET_SITE = b"flag.local" ```.
- Using Burp, changing the value of the ```Host``` header to ```flag.local``` to get the flag.

![image](https://user-images.githubusercontent.com/109911533/234905527-218f597e-954d-499d-8ca6-800910669c50.png)

**Flag: actf{reaching_the_core__chapter_8}**
