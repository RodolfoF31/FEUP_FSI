# Trabalho realizado nas Semanas #2 e #3

## Identificação

- Vulnerabilidade crítica na aplicação "Adobe Acrobat Reader" em diversas versões, incluindo 23.003.20284, 20.005.30516 e 20.005.30514. Esta aplicação é amplamente utilizada para visualizar documentos em formato PDF e representa um risco significativo tanto para empresas quanto para usuários individuais.
- Este tipo de vulnerabilidade torna-se particularmente perigoso quando combinado com técnicas de engenharia social. Por exemplo, um atacante pode se passar por um colega de trabalho por meio de um e-mail falso, levando a vítima a abrir um arquivo malicioso que explora a técnica de corrupção de memória.
-  Essa vulnerabilidade é identificada como "out-of-bound write" e permite a execução arbitrária de código, possibilitando assim a escalada de privilégios até alcançar privilégios de superusuário. 
- Devido à sua gravidade, é fundamental que essa vulnerabilidade seja abordada prontamente para evitar potenciais violações de segurança e comprometimento de dados.


## Catalogação

- A CVE-2023-26369 foi descoberta pela própria empresa Adobe Systems Incorporated.
- Este CVE tem uma pontuação CVSSv3 de 7.8, sendo considerada uma ameaça de alto risco.


## Exploit

- O exploit pode resultar na execução arbitrária de código no contexto do usuário atual. Isso poderia permitir que um invasor executasse código malicioso em sistemas suscetíveis.


## Ataques

- Devido a ser uma vulnerabilidade bastante recente e esta ter sido reportada pela própria empresa ainda não existem ataques que tenham sido reportados.

