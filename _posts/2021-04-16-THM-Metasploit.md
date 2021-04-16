# THM - Metasploit Cheatsheet

> https://tryhackme.com/room/rpmetasploit

###  Useful Commands
- **-q**

    Quiet mode start without the banner

- **msfconsole**

    Starts the metasploit command line

- **msfdb init**

    Initializes the database

- **msfconsole -h**

    Shows advanced options.

- **db_status**

    Checks if we're connected to the database and the status.
    Metasploit 5  uses postgresql.

- **?**

    Other way to look at help

- **search**

    Base command to find various modules inside of Metasploit.

- **use**
    
    To select a module as active

- **info**

    To view the info about either a specific module or the  one as  active.

- **connect**

    Netcat like function where we can coonnect to a host simply to verify that we can 'talk' to  it

- **banner**

    Displays ascii art at startup msfconsole

- **set**

    Command used to set the value of a variable

- **setg**

    Command used to set a global variable

- **get**

    Shows the content of a variable

- **unset**

    Changes the value of variable to null/no value

- **spool**

    Sets our console ouput to save to a file

- **save**

    Saves the active datastores (variables) for the next time we start Metasploit so that we don't have to keep it running in the background. To undo it just remove the created settings file.

- **advanced**

    Advanced options for a specific module

- **show**

    Shows options in a specific category

### Modules

Metasploit consists of six modules:

![THM%20Metasploit/module_diagram.png](THM%20Metasploit/module_diagram.png)

*Note, this diagram includes both the interfaces and *most* of the modules. This diagram does not include the 'Post' module.*

- **exploit**

    Holds all the exploits we're gonna use, the most common module.

- **payload**

    Used hand in hand with exploits module that contains the carious bits of shellcode we send to have executed following exploitation.

- **auxiliary**

    Module most commonly used in scanning and verification that machines are exploitable.

- **post**

    Module that helps to loot and pivot after exploit.

- **encoder**

    Commonly utilized in payload obfuscation, it allows us to modify the 'appearance' of our exploit such that we may avoid signature detection.

- **nop**

    Module used with buffer overflow and ROP attacks (Return-oriented programming is a computer security exploit technique that allows an attacker to execute code in the presence of security defenses such as executable space protection and code signing).

- **load**

    Command used to load different modules.

### Using Metasploit

- **db_nmap -sV BOX_IP**

    Runs nmap directly in Metasploit and feed it its results.

- **run -j**

    Runs the exploit as a job

- **sessions -i SESSION_NUMBER**

    Interacts with a session

- **sessions -K**

    Kills all sessions

- **migrate PID**

    Migrates our server into an other process

- **migrate -N PROCESS_NAME**

    Migrates our server into an other process

- **getuid**

    Gets information about the user running the process we are in

- **sysinfo**

    Gets info about the system itself

- ~ MIMIKATZ ~

    Mimikatz is a great post-exploitation tool written by Benjamin Delpy (gentilkiwi). After the initial exploitation phase, attackers may want to get a firmer foothold on the computer/network. Doing so often requires a set of complementary tools. Mimikatz is an attempt to bundle together some of the most useful tasks that attackers will want to perform. (https://www.offensive-security.com/metasploit-unleashed/Mimikatz/)

    In the new version of metasploit mimikatz is referred to as 'kiwi'. So to load  it we use the **load kiwi** command.

- **getprivs**

    Gets all the privileges available for our current user.

- **upload**

    Used to transfer files to our victim computer

- **run**

    Used to run a Metasploit module

- **ipconfig/ifconfig**

    Used to figure out the networking information and interfaces of our victim

- **run post/windows/gather/checkvm**

    This determines if we're in a VM

- **run post/multi/recon/local_exploit_suggester**

    This will check for various exploits which we can run within our session to elevate our privileges.

- **run post/windows/manage/enable_rdp**

    Forcing RDP to be available

*Remote desktop protocol (RDP) is a secure network communications protocol designed for remote management, as well as for remote access to virtual desktops, applications and an RDP terminal server.*

*RDP allows network administrators to remotely diagnose and resolve problems individual subscribers encounter. RDP is available for most versions of the Windows operating system. RDP for Apple macOS is also an option. An open source version is available, as well.*

*Note: Pivoting is a technique to get inside an unreachable network with help of pivot (center point). In simple words, it is an attack through which an attacker can exploit that system which belongs to the different network. For this attack, the attacker needs to exploit the main server that helps the attacker to add himself inside its local network and then the attacker will able to target the client system for the attack.*

- **shell**

    Spawn a normal system shell

- **run autoroute -h**

    Pull up the autorouting help menu

- **run autoroute -s 172.18.1.0 -n 255.255.255.0**

    Add a route to the following subnet: 172.18.1.0/24

- ~ SOCKS4A PROXY SERVER ~

    This module provides a socks4a proxy server that uses the builtin Metasploit routing to relay connections.(https://www.rapid7.com/db/modules/auxiliary/server/socks4a) 

 	

Once we've started a socks server we can modify our /etc/proxychains.conf file to include our new server. We need to prefix our commands with **proxychains** (outside of Metasploit) to run them through our socks4a server with proxychains.

More info about meterpreter: https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

- **ps**

    Lists current processes

- **execute**

    Executes a  command on the remote host

- **search**

    Finds files on the target host

- **download**

    Downloads files from the machine