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

-Navigate to the console, we will see all the command that we can execute on the website.

![image](https://user-images.githubusercontent.com/109911533/227823546-897b8e3f-5d65-45ad-a09e-a3db03749015.png)<br />

-Given a source code of the website, we can read through all of them, which we would stumble over this.<br />

![image](https://user-images.githubusercontent.com/109911533/227823815-1f86dd9f-68ac-4670-8c6f-ab0292ec1370.png)

-The ```'ping -c 3 '.$this->ip``` does a ping to the <ip>, which is our data. Without any filter, this could potential lead to a OS command injection.<br />
-Using ```;``` to end the previous command, we can now inject any command for the server to execute.
```/ping 127.0.0.1; ls -la /```<br />
- This command should return every file in the directory
  
 ![image](https://user-images.githubusercontent.com/109911533/227825146-967bdaca-a176-41bc-bf75-a6386e6daf02.png)<br />
  
-Now we just need to ```;cat /flag.txt``` to receive the flag.<br />
**flag: HTB{4lw4y5_54n1t1z3_u53r_1nput!!!}**
