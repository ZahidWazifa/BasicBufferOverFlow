
![Buffer Overflow Illustration](https://dicc.in/blog/wp-content/uploads/2021/06/buffer-overflow-attacks-min-1140x600.png)

# Buffer OverFlow Concept and practice
##  What is Buffer Overflow?
buffer overflow is a vulnerability in which a program writes more data to a buffer than it was allocated to hold. This can cause the program to crash, overwrite data, or execute malicious code. Buffer overflows are a common security issue in software, and they can be exploited by attackers to take control of a system or steal sensitive information. In general, there are 2 types of buffer overflow which are most common, namely stack overflow and heap overflow
### Stack Overflow
Stack overflow occurs when a program writes more data to a stack buffer than it was allocated to hold. The stack is a region of memory that stores local variables and function call information. When a function is called, a new stack frame is created to store the function's arguments, return address, and local variables. If a program writes more data to a stack buffer than it was allocated to hold, it can overwrite the return address or other important data on the stack. This can cause the program to crash, execute arbitrary code, or behave unpredictably. the illustration is like this
![Stack Overflow Illustration](https://hackmag.com/wp-content/uploads/2020/10/stack-layout-main-boundary-2-eng.png)

### Heap Overflow
Heap overflow occurs when a program writes more data to a heap buffer than it was allocated to hold. The heap is a region of memory that stores dynamically allocated data. When a program allocates memory on the heap, it can write data to the allocated buffer. If a program writes more data to a heap buffer than it was allocated to hold, it can overwrite other data on the heap. This can cause the program to crash, execute arbitrary code, or behave unpredictably. the illustration is like this
![Heap Overflow Illustration](https://assets.website-files.com/5ff66329429d880392f6cba2/606183718e55662036142fad_Heap%20Overflow%20Attack.jpg)


## How i Implemented the buffer overflow 
I implemented a buffer overflow using a program written in C language where there is a function in the program that will accept input continuously, causing the memory to overflow. 
For the source code itself, you can check [here](https://github.com/ZahidWazifa/BasicBufferOverFlow/blob/main/src/vuln.c)
at this segments
```diff
- char *gets(char *);

void start_level() {
  char buffer[128];
-   gets(buffer);
}
```
This function accepts all available information, although previously the amount of data that can be received has been determined. because basically this function will accept any input from users and store it in the variables provided.
 ## what do I do with this functionality?
 After knowing this, I tried to use the following steps:
1. ### Decompile Execeutable file
by using the gnu debugger (gdb) I decompile the file so that the file can be viewed using a low level programming language in terms of using assembly which represents the memory information of the stored data. the gnu debugger can be accessed at [this url](https://github.com/cgdb/cgdb)
by entering the `gdb vuln` command I decompile the executable file and see the functions that are present 
![info functions](https://drive.usercontent.google.com/download?id=1pw-OFVJCwdFhQ5RtF8-IE6rP7sDgN_3h&export=download&authuser=0&confirm=t&uuid=3d59d0ba-cb6b-4029-9a05-0b2ab24219fa&at=AIrpjvObDVZewF4c6nbUiZ4QHTKP:1739102509132)

2. ### Make and run Pattern file
at this stage I create a pattern to see what information can be received from how many files and how I can manipulate the maximum amount of memory I can put in. 
![makepattern and check ](https://drive.usercontent.google.com/download?id=1334d0Zgl38pBRfigYA9g-h-UTn4nzFSJ&export=download&authuser=0&confirm=t&uuid=c2ca2aaf-8525-469e-b3c0-34bac005ce44&at=AIrpjvMY6eQjmbSQL6oLIC-fCNi_:1739102726158)

3. ### Payload
After seeing this information I tried to insert a payload to fill all the available memory so that I could change the memory flow controlled by EIP by using an exploit that can be seen [here](https://github.com/ZahidWazifa/BasicBufferOverFlow/blob/main/src/exploit.py)
from that script we know that I'm trying to fill the value and trying to manipulate the workflow by jumping to the EIP memory address by using the [little endian](https://en.wikipedia.org/wiki/Endianness)  concept so that the workflow memory can be jumped over. after trying to enter a script which is in addition to changing the workflow I also want to enter shell code so that the programme can run the functionality like a shell in the initial stage I did shell debugging with the results: 
![result](https://drive.usercontent.google.com/download?id=1imHhstdIYdjmFwPazFbYSmVakPur34s9&export=download&authuser=0&confirm=t&uuid=565ab8b2-d2e9-40cb-ab98-71077d99c3fb&at=AIrpjvP35d8NWNlG13yvxxrkXqoI:1739103103071)

4. ### Run the script 
the next step is how I try to enter the existing shell so that I can execute the shell with the following results: 
![shellcode](https://drive.usercontent.google.com/download?id=1YYzUYtV9cdZA-lReB_YLhIEhKCOkvHS2&export=download&authuser=0&confirm=t&uuid=084f3ad0-d0bd-4eeb-a6f6-24afb196f788&at=AIrpjvONHqE9Ao8FAi_vnMqLd6uj:1739103243117)

5. ### Run Outside GDB 
The main problem is how we can run the application outside GDB, because if we try to run the application outside GDB, the script will not run like the following: 
![problem1](https://drive.usercontent.google.com/download?id=1NyeSFrnmlyQqTIEYRiS61KUAA-7HZwlo&export=download&authuser=0&confirm=t&uuid=6b0b5367-2f18-4cbc-b5b5-ec7b8fd3d6ed&at=AIrpjvP5zulAv_oC25ysYPO8WCpJ:1739103387186)
To solve this, I tried entering the command to turn off the linux security system with the following 2 commands: 
```
echo "0" > /proc/sys/kernel/randomize_va_space 
```
AND 
```
echo "core" > /proc/sys/kernel/core_pattern
```
after this command I tried to enter the next command, which is:
```
ulimit -c unlimited
```
and then i run it again to see the core Dump
![coredump](https://drive.usercontent.google.com/download?id=1nlKtMOKqId0h1Pw3eNIU7aaMpH9RN8yB&export=download&authuser=0&confirm=t&uuid=41bb9f3c-70e5-419f-8da3-ee5a0c95c8bb&at=AIrpjvPqmh2TjXIxfZ06cOf06n14:1739103627061)

6. ### Check for the Right Memory address
To run the programme I need to see the correct memory information using the gdb method and enter the core with the following command:
`gdb vuln <namacore>`
then I look at the correct memory address and put it into the payload, to see the correct information address I try to enter the command 
`x/1000wx <the topmost memory address from before>`
which result:
![result](https://drive.usercontent.google.com/download?id=1myiVTPnohZSNiPFZDsjhkCTgVqY8wC-F&export=download&authuser=0&confirm=t&uuid=72897e03-be5c-4d76-be40-83379965b50f&at=AIrpjvNEnop3Ue32BsPPAwokVpfh:1739103841450)

7. ### Run the program again with the payload
by using the existing payload I tried to run the existing programme again by entering the following command
`(cat payload.txt;cat) | ./vuln`
I managed to run the script:
![result](https://drive.usercontent.google.com/download?id=1RvgWVdQcCVDopAG3rUP4qGC55f2iicI7&export=download&authuser=0&confirm=t&uuid=106a26fc-0779-406f-b1fa-2f1910715646&at=AIrpjvNN4EpIeSMQ8b30Qorb3eTL:1739103972941)

 


