# Trabalho realizado na Semana 4

## Task 1

Utilizamos os comandos printenv ou env (pois os resultados são semelhantes) para listar as variáveis de ambiente da sessão atual do shell.Para exibir variáveis de ambiente mais específicas, Utilizamos o comando printenv PWD ou env | grep PWD.
Usamos também o comando export para definir ou modificar variáveis de ambiente e o comando unset para remover as variáveis.

## Task 2

Compilamos e executamos o ficheiro myprintenv.c, que utilizou a chamada de sistema fork() para criar um processo filho. O programa imprimiu as variáveis de ambiente do processo filho e guardamos o output num ficheiro ao usar o comando ./a.out > output_step1.txt.
Modificamos o programa ao comentar a instrução printenv() no caso do processo filho e descomentar no caso do processo pai. Esta alteração garantiu que apenas o processo pai imprimisse as variáveis de ambiente e voltei a guardar o resultado noutro ficheiro (output_step2.txt).
Utilizamos o comando diff para comparar as diferenças entre os dois ficheiros (output_step1.txt e output_step2.txt). Isto permitiu-nos determinar que não houve variações entre as variáveis de ambiente dos processos filho e pai. O que nos leva a concluir que o processo filho herda as variáveis de ambiente do processo pai.

## Task 3

Compilamos e executamos o ficheiro myenv.c, após a execução do programa observamos que as variáveis de ambiente não foram impressas, indicando que elas não foram herdadas pelo novo programa.
Depois de modificar a chamada da função execve, passando a variavel environ como terceiro argumento. Observamos que desta vez as variáveis de ambiente do processo chamador foram impressas pelo novo programa.
Com base nestes resultados, podemos concluir que as variáveis de ambiente são automaticamente herdadas pelo novo programa quando a variável environ é passada na chamada da função execve.

## Task 4

A execução deste programa imprime as variáveis de ambiente do processo de chamada (as variáveis de ambiente do seu sistema) no terminal. A função system() passa efetivamente as variáveis de ambiente atuais para o novo programa, que é o comando /usr/bin/env neste caso.

## Task 5

Criamos um programa que imprime todas as variáveis de ambiente no processo atual. Em seguida, compilamos o programa e mudamos as suas permissões para root, com as propriedades de root tornamos o programa num Set-UID. Depois, configuramos três variáveis de ambiente usando o comando export no  processo de shell atual. Ao executar o programa Set-UID, notamos que herdou todas as variáveis de ambiente que foram definidas no processo de shell. Isto significa que as variáveis foram herdadas pelo programa Set-UID mesmo quando ele é executado com privilégios elevados (root).

## Task 6

Ao modificar a variável de ambiente PATH para incluir um diretório com um programa malicioso chamado "ls," o programa Set-UID que chama system("ls") executa o código malicioso em vez do comando "ls" do sistema. Isto acontece porque o programa Set-UID confia na variável PATH e opera com privilégios, permitindo que um usuário malicioso substitua comandos seguros por comandos maliciosos.
