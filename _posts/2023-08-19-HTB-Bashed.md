# HTB - Bashed

Now that I'm back after Defcon Finals (I played in the CTF with orgakraut), I finally have the time to make this writeup for the second machine that I worked in TJ Null's list.

## Foothold

As usual, the first thing I did was run Nmap with tags letting it know to use the default scripts, enumerate the services in the ports found,  enable a verbose output, disable host discovery and save the results to a given file: `nmap -sC -sV 10.10.10.68 -vv -Pn -oN nmap/initial`.

![Initial nmap scan](/images/Bashed/nmap_initial.png)

The only port found was port 80 (HTTP). Before looking into it further, I also ran a more aggressive (`-A`) nmap scan on all ports (`-p-`): `nmap -A -p- -sC -sV 10.10.10.68 -vv -Pn -oN nmap/aggressive`.

### Port 80 enumeration

Taking a look at the homepage, I immediately saw a reference to phpbash which is "is a standalone, semi-interactive web shell" according to its [github page](https://github.com/Arrexel/phpbash).

![](/images/Bashed/home.png)

I tried looking for it in the `/uploads` folder on the website but it wasn't there. So I decided to run a gobuster search to see if there was any interesting subdirectories: `gobuster dir -u http://10.10.10.68  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,php,txt -o gobuster/initial`. As you can see I used one of the standard gobuster wordlists and added the extensions `html`, `php` and `txt`.

![](/images/Bashed/gobuster.png)

Gobuster revealed a `/dev` directory and I found the phpbash shell there: `/dev/phpbash.php`:

![](/images/Bashed/dev_files.png)

This gave me a shell on the machine:

![](/images/Bashed/phpbash_entry.png)

## Privilege Escalation 

The user flag was located in `/home/arrexel`:

![](/images/Bashed/user.png)

I decided to run `sudo -l` to see if there was anything interesting there and I saw that I could run any command as the user called scriptmanager (without a password):

![](/images/Bashed/sudo_l.png)

I decided to test it out and as you can see below, it worked:

![](/images/Bashed/test_sudo.png)

Now all I needed to do was figure out how to use this to escalate to root.

I found some interesting files in `/scripts`. It seemed that the script `test.py` (that creates a file called `test.txt`) is being executed by root because `test.txt` is shown as being created by root. So I decided to try using that to read the flag contents in `/root` and write them to a file we have access to. (Note: I wanted to do that to keep it simple, but I could have also tried other ways that would have let me actually become the root user)

I struggled to be able to modify the file using `sudo -l` as scriptmanager so I decided to instead change the perms so that I could modify them as my current user `www-data` using the two following commands:

- `sudo -u scriptmanager chmod 777 /scripts`

- `sudo -u scriptmanager chmod 777 /scripts/test.py`

I now added the following to `test.py` (with commands looking like this: `echo line >> test.py`). The script opens `root.txt` and then copies the content to our new file `tt.txt`:

```python
f = open("/root/root.txt", "r")
data = f.read()
f.close
f = open("tt.txt", "w")
f.write(data)
f.close
```

![](/images/Bashed/test_py_modif.png)


Then I was able to just read the `tt.txt` file and get the root flag:

![](/images/Bashed/root_txt1.png)