# 1. Challenge name

GDB baby step

## Solution:

1. During the citadel ctf reverse engineering challenge(tameimpala) I had got introduced to gdb even tho it wasn't the right tool for that challenge. Reading the challenge name I knew I had to use gdb for this task.
2. I already knew basic commands like setting break points and `disas`(disas disassembles the particular breakpoint).I learnt some new commands. I primarily used gemini ai for learning the new commands such as `p/d $eax`.
3. Firstly I made the file executable.
```bash
akg451@akg451-OMEN-by-HP-Gaming-Laptop-16-xd0xxx:~/Downloads$ chmod +x debugger0_a
```
After this I ran the file in gdb using `gdb ./debugger0_a` which is basically `gdb ./file_name`
```bash
akg451@akg451-OMEN-by-HP-Gaming-Laptop-16-xd0xxx:~/Downloads$ gdb ./debugger0_a
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./debugger0_a...
(No debugging symbols found in ./debugger0_a)
```
Now I set my first breakpoint in the main function. The reason I did that is because in the question it is clearly mentioned that the eax register existed in main fuction.After setting the breakpoint I ran the functions. What this will do this time is that it will stop as soon as it reaches main function since we have set a breakpoint there. 
```bash
(gdb)  b main
Breakpoint 1 at 0x1131
(gdb) r
Starting program: /home/akg451/Downloads/debugger0_a 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x0000555555555131 in main ()
```
Now after this I decided to disassemble the function and got the below output.
```bash
(gdb) disas
Dump of assembler code for function main:
   0x0000555555555129 <+0>:	endbr64 
   0x000055555555512d <+4>:	push   %rbp
   0x000055555555512e <+5>:	mov    %rsp,%rbp
=> 0x0000555555555131 <+8>:	mov    %edi,-0x4(%rbp)
   0x0000555555555134 <+11>:	mov    %rsi,-0x10(%rbp)
   0x0000555555555138 <+15>:	mov    $0x86342,%eax
   0x000055555555513d <+20>:	pop    %rbp
   0x000055555555513e <+21>:	ret    
End of assembler dump.
```
This where I hit a roadblock. I thought initially that I would see a `eax` function immediately. I have explained the roadblock I hit in the `notes` section of this md file. But moving forward I set another breakpoint at `ret` instruction and continued by the command `c`. Finally I read the value of eax register using the command `p/d $eax`. What this command does it tells to print the value as a decimal number.
```bash
(gdb) b *0x000055555555513e
Breakpoint 2 at 0x55555555513e
(gdb) c
Continuing.

Breakpoint 2, 0x000055555555513e in main ()
(gdb) p/d $eax
$1 = 549698
```

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- p/d command in gdb
- eax register
- 

## Notes:

1. The tangent I hit is that I ran the below code directly:
```bash
Dump of assembler code for function main:
   0x0000555555555129 <+0>:	endbr64 
   0x000055555555512d <+4>:	push   %rbp
   0x000055555555512e <+5>:	mov    %rsp,%rbp
=> 0x0000555555555131 <+8>:	mov    %edi,-0x4(%rbp)
   0x0000555555555134 <+11>:	mov    %rsi,-0x10(%rbp)
   0x0000555555555138 <+15>:	mov    $0x86342,%eax
   0x000055555555513d <+20>:	pop    %rbp
   0x000055555555513e <+21>:	ret    
End of assembler dump.
(gdb) b *Quit
(gdb) b *0x0000555555555138
Breakpoint 2 at 0x555555555138
(gdb) c
Continuing.

Breakpoint 2, 0x0000555555555138 in main ()
(gdb) p/d $eax
$1 = 1431654697
```
This method is not right since this doesn't get us the flag because that's not how eax works.
2. Now what I did is I first learnt about the eax function. The conversation I had with gemini regarding it is as follows. Also I watched some videos to learn what exactly is eax. Finally after all this I got to know that eax register works hand-in-hand with ret instruction. Hence if I want the correct output I will have to print the `ret` intruction and not `mov`. This is what I did and finally got the flag.
https://gemini.google.com/share/d6db3d034857 

## Resources:

- gemini: https://gemini.google.com/share/d6db3d034857
- youtube: https://youtu.be/J2WAwqv2-T0?si=O6CyOY6rANYYJFlI, https://youtu.be/5SICv-2tMgQ?si=_GEFSkbMLvuiYCIB
- stackoverflow: https://stackoverflow.com/questions/29689014/return-value-eax-convention


***

# 2. Challenge name

> Description

.
.
.