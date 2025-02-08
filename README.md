
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
 


