# Ring Zer0 CTF - Looking for Password file

This challenge is part of the web section of the RingZer0 CTF and can be found [here](https://ringzer0ctf.com/challenges/75).

On a first look, the home section of the webpage of this challenge looks pretty bland:

![Password%20File/webpage.png](/images/Password%20File/webpage.png)

However its URL is very interesting: `http://challenges.ringzer0team.com:10075/?page=lorem.php`

We can see a `page` parameter and it seems that it will display the given page. The vulnerability here is LFI (local file inclusion). We will try to display a file on the machine that we should not have access to normally (I'm guessing it's a password file if we trust the challenge's name). 

I decided to test this out and passed through `flag.php` (the url now being: `http://challenges.ringzer0team.com:10075/?page=flag.php`) and I got back the following error:

![Password%20File/error.png](/images/Password%20File/error.png)

We can see that `flag.php` does not exist, however this error also helps us verify that the website is located in `/var/www/html/`. Since the name of the challenge is "Looking for password file", we can guess that the file we are looking for is `/etc/passwd`. So we are going to the following url: `http://challenges.ringzer0team.com:10075/?page=../../../etc/passwd`.

And just like that we get the password file and with it the flag!

## Contact
If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.