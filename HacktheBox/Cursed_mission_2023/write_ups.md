# SOME WEB CHALLENGES WRITE UPS #

### 1. Trapped Source ###
- Read the soure code, we will find the secret PIN.<br/ >
```html
<script>
    window.CONFIG = window.CONFIG || {
        buildNumber: "v20190816",
        debug: false,
        modelName: "Valencia",
        correctPin: "8291",
    }
</script>
```
- Now that we got the pin, enter it to the safe-like thing, and get the flag.
**flag: HTB{V13w_50urc3_c4n_b3_u53ful!!!}**

### 2. Gunhead ###
#### Overview
![image](https://user-images.githubusercontent.com/109911533/227821590-4271f2e8-c0a8-4757-94e1-6cc0ca2a5dc6.png)<br />

- Navigate to the console, we will see all the command that we can execute on the website.

![image](https://user-images.githubusercontent.com/109911533/227823546-897b8e3f-5d65-45ad-a09e-a3db03749015.png)<br />

- Given a source code of the website, we can read through all of them, which we would stumble over this.<br />

![image](https://user-images.githubusercontent.com/109911533/227823815-1f86dd9f-68ac-4670-8c6f-ab0292ec1370.png)

- The ```'ping -c 3 '.$this->ip``` does a ping to the <ip>, which is our data. Without any filter, this could potential lead to a OS command injection.<br />
- Using ```;``` to end the previous command, we can now inject any command for the server to execute.
```/ping 127.0.0.1; ls -la /```<br />
- This command should return every file in the directory
  
 ![image](https://user-images.githubusercontent.com/109911533/227825146-967bdaca-a176-41bc-bf75-a6386e6daf02.png)<br />
  
- Now we just need to ```;cat /flag.txt``` to receive the flag.<br />
**flag: HTB{4lw4y5_54n1t1z3_u53r_1nput!!!}**
    
### 3. Drobots ###
- This challenge is pretty simple, if you've learned about SQLi, then you should notice there's a SQLi vulnerability here, in the database.py file.<br />
    
![image](https://user-images.githubusercontent.com/109911533/228113328-5b909aa5-01fd-436b-b525-f4d472d2cbfd.png)<br />
    
- Using ```admin" -- ``` should do the trick.<br />
**flag: HTB{p4r4m3t3r1z4t10n_1s_1mp0rt4nt!!!}**

### 4. Passman ###
- This challenge is an IDOR vunerability chall.<br />
- Reading through all the file i noticed that in the ```GraphqlHelper.js```, all the function forrmat are the same. And also, there's a function called UpdatePassword, maybe we can do something with it??<br />
    
![image](https://user-images.githubusercontent.com/109911533/228114651-ba3ec6a1-dacf-46b1-b3ff-063c08b6a13f.png)<br />

- Using BurpSuite, we can first use the addphrase function in order to get the format of the query. Then, edit it using the UpdatePassword variables, turning it into an UpdatePassword function, which should allow us to change the admin password.<br />

![image](https://user-images.githubusercontent.com/109911533/228115190-6f67bc26-fa02-44d2-8e6d-c8d700e8dbf5.png)<br />

- Change the query into this.<br />
```{
  "query": "mutation($username: String!, $password: String!) { UpdatePassword(username: $username, password: $password){ message } }",
  "variables": {
    "username": "admin",
    "password": "123"
  }
```
- This should change the admin password to 123, and we can verify it with the response.<br />

![image](https://user-images.githubusercontent.com/109911533/228115508-cdd021d3-3795-43b9-94f7-27d46c90a0dd.png)<br />
 
- Login as admin, we get the flag.<br />
**flag: HTB{1d0r5_4r3_s1mpl3_4nd_1mp4ctful!!}**
    
 ### 5. Orbital ###
- For this challenge, i think there are more simple ways to do it than i did, but i cant think of anything so i chose to bruteforce the password.<br />
- The password is MD5 hashed so at first i think i need to hash all the password to MD5, but no, the web page will take my password input, hash it, and compare it to the original password. I chose ```rockyou.txt``` for this one since it HacktheBox and ```rockyou.txt``` contains the most common passwords.<br />
- Here is the scripts.<br />
```py
import requests

url = "http://68.183.37.122:32168/api/login"

with open("rockyou.txt", errors="ignore") as file:
    passwords = [l.strip() for l in file.readlines()]

    for password in passwords:
        print('Testing password: "{}"'.format(password))

        data = {'username': 'admin', 'password': password}
        response = requests.post(url, json = data)

        if "Invalid" not in response.text:
            print(response.text)
            print("Password found: {}".format(password))
            break
```
- Running it for some times, should return the password, which is ```ichliebedich```.Now we login as admin.<br />

- Now we need to get the flag file. But the flag file is not in the ```flag.txt``` or anything else, but its in this file.<br />
![image](https://user-images.githubusercontent.com/109911533/228118532-d683962a-1036-48bf-be30-20d1a9f0b2a1.png)<br />
    
- Reading through all the source code again, you notice in the ```routes.py``` there's a funtion ```export```, which export a variables ```{communicationName}``` which is a variables that we can change, so maybe path traversal exploit?<br />
    
![image](https://user-images.githubusercontent.com/109911533/228118879-7114b43f-6767-4a27-bde5-aae25c2b9e7f.png)<br />

- Using burpsuite, we should be able to retrieve the flag.
    
![image](https://user-images.githubusercontent.com/109911533/228119016-788d73bd-521e-4d94-898a-b63baccbcdd8.png)<br />
    
- P/s: I think this chall was supposed to be time based SQLi, not brute forcing random sh*t xDDD, but oh well...
    
**flag: HTB{T1m3_b4$3d_$ql1_4r3_fun!!!}**

### 5. Didactic Octo Paddles ###
- This challenge is about JWT token and Node.js SSTI.<br />
- First, looking through the given source code, we can see there's a route called ```/admin``` which may give us access to the admin page.<br />
- Register and Login, you should see a session cookie which is a JWT Token, decode it return something like this.<br />
    
 ![image](https://user-images.githubusercontent.com/109911533/228418179-fa77d3b2-bb6c-476d-86bd-3b2636ad2715.png)

- Now in the file ```AdminMiddleware.js```, we can see its checking for the ```alg``` and ```user.id```. If success, it'll return us the admin page.<br />
- If the ```alg==HS256```, it will return us as user, whether ```alg==none``` will redirect us back to the login page. So to bypass this, we can you a varieties form of ```none```, like ```None, nOne, NoNe, NONE,...```. This should bypass the ```alg``` checking function.<br />
- Now, for the ```user.id```, its very common for these type of challenges that the admin ```id``` will be ```1```, since admin is the first guy to create and added to the database. So now, we just need to change ```alg:NONE``` and ```id:1```. This should redirect us to the admin page.(Remember after changing the two variables, delete the last part of the payload).<br />
- The JWT token should now be<br />
```eyJhbGciOiJOb25lIiwidHlwIjoiSldUIn0.eyJpZCI6MSwiaWF0IjoxNjc5NTYwOTYwLCJleHAiOjE2Nzk1NjQ1NjB9.```<br />
    
![image](https://user-images.githubusercontent.com/109911533/228421571-c3b1e49a-96b8-4311-a1e3-162a2873d07b.png)<br />

- Now we should be able to see the admin page, here i login as user ```a```<br />
![image](https://user-images.githubusercontent.com/109911533/228420867-6ab8aec0-d8c8-476f-8154-ce2737f5b27a.png)

- So, now what do we do to get the flag? If you read the code about the ```/admin``` route, you will see they are using ```jsrender.templates```. If you read the document about it, you'll know that you can pass in template expressions and it will process them, then output it to the screen. Here the ```jsrender.templates``` is using the variable ```username```. So we can confirm that there's a JsRender Node.js SSTI vulnerability in ```username```.<br />
- Reading about it, i found this playload that we can use ```{{:"pwnd".toString.constructor.call({},"return global.process.mainModule.constructor._load('child_process').execSync('cat /flag.txt').toString()")()}}```<br />
- Now we take that payload, register it as a user, and go to the admin page to get the flag.<br />
    
![image](https://user-images.githubusercontent.com/109911533/228422188-d4093b4f-3092-455e-b388-70d78b538700.png)<br />


    
 




