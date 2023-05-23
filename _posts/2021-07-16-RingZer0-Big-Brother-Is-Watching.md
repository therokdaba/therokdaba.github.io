# Ring Zer0 CTF - Big Brother Is Watching

This challenge is part of the web section of the RingZer0 CTF and can be found [here](https://ringzer0ctf.com/challenges/212).

The challenge page shows the following:

![Big%20Bro/chall_page.png](/images/Big%20Bro/chall_page.png)

There is a reference to Google, this can make you think of Google crawlers and lead you to check the site's `robots.txt` file. In fact, *a robots.txt file is used primarily to manage crawler traffic to your site, and usually to keep a file off Google* (definition found here: [https://developers.google.com/search/docs/advanced/robots/intro](https://developers.google.com/search/docs/advanced/robots/intro)). So the `robots.txt file` could make sure that Google does not find this challenge's flag.

In fact, if we go to [https://ringzer0ctf.com/robots.txt](https://ringzer0ctf.com/robots.txt), we find the following directory: `/16bfff59f7e8343a2643bdc2ee76b2dc/`. Now, if we follow this directory and go to [https://ringzer0ctf.com/16bfff59f7e8343a2643bdc2ee76b2dc/](https://ringzer0ctf.com/16bfff59f7e8343a2643bdc2ee76b2dc/), we get the flag!

## Contact
If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.