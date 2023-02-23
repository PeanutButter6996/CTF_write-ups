![image](https://user-images.githubusercontent.com/109911533/220856284-9cd886d2-98df-417d-8453-33e8ef317771.png)<br />

This challenge for me is so misleading xD, cost me so much time but still ... i got it.<br />
Lets take a look at the website.<br />
![image](https://user-images.githubusercontent.com/109911533/220856885-118d6b37-1476-44f0-b1e7-e6262b74bbfd.png)<br />

Hmmmm, nothing much too see. How about taking a look at the page's source. <br />
![image](https://user-images.githubusercontent.com/109911533/220857098-02bba6cb-f1df-45ea-be9c-4d30c8e0e87c.png)<br/>

Looks like there's a hidden page called "/hidden_page", lets visit that.<br />
![image](https://user-images.githubusercontent.com/109911533/220857412-c74f17f5-60d7-40d5-8473-e2e98032cc7e.png)<br />

 The page told us that we need a key, but where you ask? This is the part that it got me so mislead that i ended up brute forcing for more than an hour.Basically the admin gave us this hint.<br />
![image](https://user-images.githubusercontent.com/109911533/220857773-df6c222e-0e28-47b8-a259-4690d283b2b3.png)<br />

Ahhh, classic rockyou.txt. If you dont know about it, you can look it up online about what it is.<br />
So now, what do we do in order to get the key. Brute forcing of course. Lets build a script to brute force the key from the website.This is a simple python script that should do the work.<br />
![image](https://user-images.githubusercontent.com/109911533/220858536-b99833ec-e726-4287-a16e-465ee1e964f8.png)<br />

So now we run the script, and start the waiting game. This is where i started to doubt myself about if its the correct way to do it since everyone in the Discord started to talk about it (no hints were given of course). But basically everyone is saying no need to brute force the key what so ever, and maybe there was something else, or you brute forcing the wrong part, bla bla ,...<br />

An HOUR passed without any result from the script, so i think there must be something wrong with the script or maybe there was something else. Waiting for the script i started to look at the source code one more time, just find out the infamous "key" is in a CSS file.<br />
![image](https://user-images.githubusercontent.com/109911533/220859876-d6d57d9b-cc53-4d34-bdeb-28afa656e5a2.png)<br />
![image](https://user-images.githubusercontent.com/109911533/220860016-76bd3dec-3841-4a82-8fb9-2a4b7a2a76d2.png)<br />

LOL, i stop the script and wanted to cry xD. Like for real? OMG.Add it to the url and we got it.<br ?>
![image](https://user-images.githubusercontent.com/109911533/220860307-a230779e-979f-4add-bfe3-847bfc36378a.png)<br />

It direct us to a page and said that we need to verify as ADMIN, which of course we're not. By clicking on verify we got "SUS" by the website.<br />
![image](https://user-images.githubusercontent.com/109911533/220860663-24a91f2f-41a9-4447-b201-5c8b8800e7bf.png)<br />

Now if we take a look at the url, there's a key parameter saying "/?key=anotherkeylol". So you think this will be where we brute force to get the key for admin right?<br />

NO!!!

Again everyone is talking about no need to brute force the key parameter. So maybe it's something else.<br />
This is where i got stuck. I basically dont know what to do since you need to be the admin in order to be able to verify, and the url said that we need another key, if it's not brute forcing the key, what else coudl it be.<br />
Wandering around the chall'page, i notice something. There's a session cookie. Since a lot of web challs tend to be build on Flask, i grab that cookie and decode it. And voila, the cookie is setting "admin":"false".<br />
![image](https://user-images.githubusercontent.com/109911533/220862506-9d110cad-8ac7-4d35-a23c-2e02c06589be.png)

Now we know that to do next, which is to bake a Flask cookie for the website with "admin":"true" to able to become the admin and verify. But if you notice, when forging a flask cookie, we need a secret key.<br />
So this is where brute forcing comes in, using rockyou.txt we need to brute force the secret key, then use it to encode the cookie with the admin value setting to true. <br />
This is my script for it.<br />
```python
import requests
import sys
import zlib
from itsdangerous import base64_decode
import ast


from flask.sessions import SecureCookieSessionInterface

class MockApp(object):
    def __init__(self, secret_key):
        self.secret_key = secret_key


def encode(secret_key, session_cookie_structure):
    """ Encode a Flask session cookie """
    try:
        app = MockApp(secret_key)
        session_cookie_structure = dict(ast.literal_eval(session_cookie_structure))
        si = SecureCookieSessionInterface()
        s = si.get_signing_serializer(app)

        return s.dumps(session_cookie_structure)

    except Exception as e:
        return "[Encoding error] {}".format(e)
        raise e

url = "https://chocolates-mhsctf.0xmmalik.repl.co/admin-check"

payload = {"admin": "true"}

#with open("rockyou.txt", errors = "ignore") as file:
    #lines = [l.strip() for l in file.readlines()]
    #for secret_key in lines:

print(encode("BATMAN", "{'admin':'true'}"))
        #print('Testing secret key "{}"'.format(secret_key))
        #r = requests.get(url , cookies = { "session" : new_cookie })

        #if "valentine{" in r.text:
            #print("GOT ITTTTTTTTTTTTTTTTTTTTTTTTTTTT")
            #break
```<br />
The # comment i use is when i got the secret key, which is "BATMAN", then i comment all the code out, and use the encode function to encode "BATMAN" with admin setting to true.<br />
![image](https://user-images.githubusercontent.com/109911533/220864698-c3f3b177-97a8-422e-a1ed-04c66fffc086.png)<br />

Now we got that, lets copy and paste it on to the website cookie, and there you have it, the FLAG.<br />
![image](https://user-images.githubusercontent.com/109911533/220864973-977ec71f-fcc7-4d76-8a93-2f70cdd1171c.png)









