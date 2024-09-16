# Desafio 1

Utilizando o comando checksec verificamos que program tem arquitetura x86 (Arch), não existe um canário a proteger o return address (Stack), a stack tem permisssão de execução (NX), e as posições do binário não estão randomizadas (PIE), por fim existem regiões de memória com permissões de leitura, escrita e execução (RWX), neste caso referindo-se à stack.
No código de main.c, observamos que há uma alocação de 9 bytes de memória para a variável meme_file, que armazena o nome do ficheiro, e uma alocação de 32 bytes para a variável buffer, que é usada para armazenar o input utilizador. Observamos tambem que a função scanf permite a cópia de até 45 bytes do stdin para o buffer. Essa configuração pode resultar num buffer overflow quando a entrada do utilizador excede o tamanho do buffer, que é de 32 bytes.

```c
char meme_file[9] = "mem.txt\0\0";
    char val[4] = "\xef\xbe\xad\xde";
    char buffer[32];

    printf("Try to unlock the flag.\n");
    printf("Show me what you got:");
    fflush(stdout);
    scanf("%45s", &buffer);
```


Conforme o funcionamento da stack, a memória alocada é contígua e depende da ordem em que as variáveis são declaradas. Portanto, se excedermos a capacidade do buffer, acabamos por sobrescrever a área de memória dedicada à variável meme_file.
Assim, basta fornecer como input o seguinte valor "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxflag.txt" e temos acesso à flag.

# Desafio 2

Utilizando o comando checksec verificamos que program tem arquitetura x86 (Arch), não existe um canário a proteger o return address (Stack), a stack tem permisssão de execução (NX), e as posições do binário não estão randomizadas (PIE), por fim existem regiões de memória com permissões de leitura, escrita e execução (RWX), neste caso referindo-se à stack.
No código de main.c, observamos que há uma alocação de 9 bytes de memória para a variável meme_file, 4 bytes para o valor val e uma alocação de 32 bytes para a variável buffer, que é usada para armazenar o input utilizador.

```c
char meme_file[9] = "mem.txt\0\0";
char val[4] = "\xef\xbe\xad\xde";
char buffer[32];
```

Este desafio é semelhante ao primeiro, a diferença é que só é possível aceder ao ficheiro quando o valor de val é 0xfefc2324. Após, correr o programa verificamos que o conteúdo inicial de val é 0xdeadbeef, através desta informação reconstruimos os bytes de val para obtermos o resultado desejado ("\x24\x23\xfc\xfe").
Assim, basta fornecer como input o seguinte valor "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\x24\x23\xfc\xfeflag.txt" e temos acesso à flag.

