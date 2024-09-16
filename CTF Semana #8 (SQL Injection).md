# Semana 8 (SQL Injection)

Neste desafio, acedemos ao servidor (https://ctf-fsi.fe.up.pt:5003/) em que temos um form de login onde é possível inserir um username e uma password. É nos disponibilizado um ficheiro (index.php) que corre do lado do servidor, após analisarmos o código verificamos que a flag.txt é apresentada quando iniciamos sessão, a query para o inicio de sessão é a seguinte :

```sql
$query = "SELECT username FROM user WHERE username = '".$username."' AND password = '".$password."'";
```

Assim, quando fornecemos o input admin'--, utilizamos o username admin pois é o que é usado na task 2 do guião de SQL injection, a verificação da password é comentada e esta torna-se irrelevante, porém temos de inserir uma password aleatória pois é um campo obrigatório. Desta forma, iniciamos sessão com o admin e obtemos a flag do desafio.
