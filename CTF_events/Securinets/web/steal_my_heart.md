---STEAL MY HEART---<br />
A web chall from Securinets Love Bug CTF<br />
(Since the website for the chall is down, so i'll do it locally and try to explain it to ya xD)

The chall give us a source code, let's take a look
![Screenshot 2023-02-22 120952](https://user-images.githubusercontent.com/109911533/220529541-c1319284-ab82-4fb5-9d12-44083724b46c.png)<br />

Looking at the code, we can get the basic concept of what the website is doing, basically, its setting the variable "secret" to a variable name "SECRETT" which it gets from the os.environ command<br />

It then get the length of that "SECRETT", and generate a random number between 1 and the length of that secret string.

After that, it will execute this command,<br />
![Screenshot 2023-02-22 122438](https://user-images.githubusercontent.com/109911533/220530452-9114265d-f712-4a2d-89aa-e147bafc97cc.png)<br />

This command set the width to x, meaning if the website generate the corrent number, it will then shorten that secret string, adding the placeholder "nope" at the end. Basically, secret+nope at the end. However, there's a vunerability in here. With the help of ChatGPT xD, i know that, if the exact number is generated, then it will print out the secret for us without the placeholder nope at the end.<br />

So now we got that in mind, lets take a look at the website(since the website is down, i deploy it locally, but the problem is i dont know what is the secret, so i'll have to change the secret to something else, but it doesn't really affect that much)
![image](https://user-images.githubusercontent.com/109911533/220532386-558a7aab-e20e-4ab8-8dd7-9dc94141219f.png)<br />
![image](https://user-images.githubusercontent.com/109911533/220532548-65eb5fde-1c9d-4176-9d9e-cb960f911339.png)<br />

Here, i set the secret to "hello", and after refreshing the page for about 5 times, i got it<br />
![image](https://user-images.githubusercontent.com/109911533/220532707-500c19ff-19d4-4db6-a139-76b6cd2dc28c.png)<br />

Now we know how the website works, but we dont know what's the actual secret is. This is where i look at the hint. There was 2 hints, both is about debugger and rce, and also the source code shows us that the site is set to debugger true.<br />
Looking up online for "FLASK RCE DEBUGGER TRUE", i got this website "http://ghostlulz.com/flask-rce-debug-mode/"<br />
Read through it, i now know that there's a page called /console, but it require what so called a PIN, and when deployed it locally, we can see that it does generate a debug PIN<br />
![image](https://user-images.githubusercontent.com/109911533/220533843-f0e9919e-5219-4aa2-9acd-f2d1e179d795.png)<br />
At this point, if you actually go to the actual webpage and reload for sometimes, it will generate out a PIN, and you can use it to get access to the Werkzueg Console. Now we just need to rce the console to get the flag<br />
![image](https://user-images.githubusercontent.com/109911533/220534622-6437b6eb-afc7-4f6f-be5d-675fc33c7308.png)<br />

Now, by replacing ls with cat flag.txt, we got the flag.<br />

___Flag: Securinets{RCE_My_heart_all_yours!!}___





