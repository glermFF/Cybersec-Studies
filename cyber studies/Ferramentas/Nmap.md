Doc: https://nmap.org/
**Equipe Destinada: [[Blue Team]] [[Red Team]]

---

## Sobre

Ferramenta para análise de vulnerabilidade e coleta de informações de aparelhos conectados na rede de computadores. Por meio dele podemos fazer varredura nas portas de comunicação de um determinado aparelho, verificando seu estado para caso haja uma brecha, tratar dela (ou se beneficiar do exploit). 

## Modos de operação

**Varredura simples:**
```bash
nmap 192.168.0.1
```

**Varredura sem ping:** faz o scan assumindo que o host está conectado na rede. Usado para confirmar se o host no qual sabemos estar online na rede foi dado como offline em um scan anterior

```bash
nmap -PN 192.168.0.1
```
 

**Varredura ping sweep:** envia o pacotes ICMP para todo host na rede para identificar quais estão ativos e quais não estão
```bash
nmap -sn 192.168.xxx.xxx/24
```


**Descobrindo hosts:** para fazer uma varredura na rede a procura da quantidade de aparelhos que estão conectados nela, podemos fazer isso pela flag *-sP*  que faz um ping na rede "perguntando" para todos os dispositivos "Quem é você?"
```bash
nmap -sP 192.168.xxx.xxx/24 
```

**TCP scan completo:** para fazer o scan das portas de um host seguindo o "aperto de três mãos" (three way handshake | [[Redes de Computadores]]) usamos o comando *-sT* para tal. Comando *-p* é usado para especificar as portas que queremos analisar

*Pode ser necessário utilizar o sudo para efetuar o scan*

Host
```bash
nmap -sT -p 80, 433 192.168.xxx.xxx 
```

Rede: 
```bash
namp -sT -p 80, 433 192.168.xxx.xxx/24
```

**UDP Scan:

**Stealth Scan:** ou varredura SYN, onde a última etapa do three way handshake não é efetuada
```bash
nmap -sS 192.168.xxx.xxx
```

**Detecção de SO:** a detecção do sistema operacional de um host pode facilitar a guiar um ataque auxiliando em quais vulnerabilidades podem ser aproveitadas. Usamos o comando *-O*. Requer permissão do admin user. 
```bash
nmap -O 192.168.xxx.xxx
```

**Agressividade nos scans:** agressividade aqui quer dizer o nível de cuidado que o nmap vai tomar na varredura para não chamar tanta atenção de sistemas de detecção de segurança monitorando a rede. Usamos o comando *-T* e os níveis variam de 0 a 5
```bash
nmap 192.168.xxx.xxx -T 4
```

**Scripts:** podem ser feitos scans utilizando scripts para fazer uma varredura automatizada. Podemos usar scripts personalizados ou usar os disponíveis no site do nmap.

Dependendo da combinação de comandos que for feita em um scan e principalmente se este scan for feito para toda a rede, pode ser que demande um grande tempo de execução. Se chegar a bater 10 minutos de espera, termine a execução e faça outro scan. Para verificar o tempo de execução use CTRL + T.



---