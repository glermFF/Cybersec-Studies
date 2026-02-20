### Curl

Curl, um poderoso comando bem interessante com funcionalidades voltadas a transferência de arquivos entre hosts e serviços web. Versátil, podendo ser utilizada em qualquer tipo de plataforma/SO e muito fácil de ser utilizado. Utilizando as combinações de flags bem certas podemos ter comandos poderosos que conseguem acessar, verificar status do servidor, verificar url feitas por encurtadores e efetuar transferências de grandes volumes de dados dentro de um curto espaço de tempo. Chega ser bem próximo de uma ferramenta para o computador se analisarmos bem.

Seus comandos podem ser combinados com API's voltadas especialmente para o uso do curl. Dẽ uma olhada no seguinte link ---> [No auth API's](https://mixedanalytics.com/blog/list-actually-free-open-no-auth-needed-apis/)

**EX: Previsão do Tempo via CLI**

*entrada:*
```bash
curl https://wttr.in/Brasilia 
```

*saída(incompleta):*
```bash
Weather report: brasilia

     \  /       Partly cloudy
   _ /"".-.     21 °C          
     \_(   ).   → 4 km/h       
     /(___(__)  10 km          
                0.0 mm         
```

**Comandos úteis:**

*nada >> imprime página do site:
```bash
curl example.com
# use -v para retorno de mais detalhes sobre o site
curl -v example.com
```

*-o >> baixar arquivos:
```bash
curl -o http://example.com/image.png
```

*-X GET ou --get >> envia requisição GET 
```bash
curl -X GET http://example.com
```

---

*referências:*
- [Everything curl (Site)](https://everything.curl.dev/)
- [Wait... what else curl can do?](https://www.youtube.com/watch?v=3xmD4E2aqxo)
- [Curl Videos](https://curl.se/docs/videos/)