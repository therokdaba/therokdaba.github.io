# Ring Zer0 CTF - Words mean something?

This challenge is part of the web section of the RingZer0 CTF and can be found [here](https://ringzer0ctf.com/challenges/42).

This challenge's page shows the following:

![Words%20Mean%20Something/chall_page.png](/images/Words%20Mean%20Something/chall_page.png)

First thing I did was check the site's source page but sadly it didn't give out any flag or any interesting information. So I then took a look at Firefox's Web Developer Tools. 

There I found a flag cookie with a value of `0` (*Storage->Cookies->https://ringzer0ctf.com*). I then changed its value to 1 and reloaded the page...and voilà the flag is now displayed instead of the previous text!

## Contact
If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.