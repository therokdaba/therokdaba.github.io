# CTFLearn - Basic Injection

This challenge is an easy challenge on the CTFLearn's platform and can be found [here](https://ctflearn.com/challenge/88).

It has the following prompt:

```
See if you can leak the whole database using what you know about SQL Injections. 
```

So we already know that we need to use SQL Injection and looking at the challenge's name, it's going to be a pretty basic one.

So let's go to this website:

![Basic%20Injection/webpage.png](/images/Basic%20Injection/webpage.png)

If we enter `' or 1=1; -- -`, we are able to leak the whole database.

For those of you who don't really know how SQL Injections work, this exploit works this way:

- first we use a quote so that the name supplied is blank

- and we then use `or 1=1;` to break the selection, basically now it will select every item of the database if it's name is either blank or if 1 is equal to 1 which is always the case, so it will leak the whole database.

- `-- -` we use this to comment out the rest of the query, here the comments are used to make sure that the following part of the original query isn't parsed by the database and the third dash is used to make sure that the browser doesn't remove the trailing space after the first two dashes. 

## Contact
If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.