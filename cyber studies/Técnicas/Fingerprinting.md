Esta etapa consiste em coletar informações mais detalhadas sobre os dispositivos na rede encontrados anteriormente na etapa de [[Footprinting]] que fizemos para entender quem está compondo a estrutura de rede que estamos analisando. Faremos tudo isso por métodos de escaneamento de dispositivos, vulnerabilidades, serviços, informações sobre pessoas, etc.

No scan podem ser usadas diversas ferramentas que conseguem efetuar tal ação. [[Nmap]], hping3, fping, arping são algumas delas e que vamos ver neste módulo.

---
### Tipos de Fingerprinting

#### Passivo:

- Na forma passiva do Fingerprinting, são utilizadas ferramentas que colocam a máquina na posição de analisar o que está conectado na rede. Quando algum dispositivo "forasteiro" acessa a rede, fazemos uma comparação para saber se esse dispositivo já esteve em algum momento conectado por ela pela comparação da base de dados na rede.
- As ferramentas aqui geralmente são utilizadas em servidores ou dispositivos que estão transmitindo serviço para outros dispositivos.
- Ferramenta: p0f.
#### Ativo: 

-  Na forma ativa é uma maneira mais direta de conhecer os dispositivos. Um problema a ser enfrentado é com relação a mecanismos/sistemas de defesa na rede, pois estamos mandando pacotes que vão trafegar entre os intermediadores que conectam você com a máquina alvo e isso chama atenção.
-  Há um leque de ferramentas para a forma ativa, desde pré instaladas no sistema operacional e algumas com mais recursos que precisaremos instalar por terceiros.
- Ferramentas pré instaladas: ping, traceroute
- Ferramentas de terceiros: fping, hping3, arping

---

## Coleta de Informações em Websites

Acessar um site é a forma mais simples de conectar com uma aplicação. basta ter internet, pesquisar o endereço do site na url e pronto você entrou. E como toda aplicação digital, sites também são propensos à vulnerabilidades e falhas. Por aqui veremos as formas e ferramentas de analisar falhas em aplicações e serviços na web.

### WHOIS

Ao hospedar um site e registrar um domínio na web deve-se registrar este domínio seguindo a regra da IANA onde serão salvos os dados básicos do site, a organização que o administra, IP, serviços utilizados, se há outros domínios linkados ao site, etc. No Brasil, o órgão que disponibiliza essas informações dos domínios nacionais é o Registro.br (https://registro.br/tecnologia/ferramentas/whois/).

### Netcraft

Por esse site, podemos verificar as mesmas informações como se usa-se o Whois, o porém aqui é a possibilidade de verificarmos as versões que o site já teve e quais tecnologias utilizam. Há mais informações detalhadas relacionadas a seguranças.

### Informações sobre DNS

*Crie uma conta para ter acesso há outros tipos de dados*

https://www.robtex.com/

### Sites no mesmo servidor

Servidores podem hospedar mais de um site dentro deles, como um computador pode possuir mais de um programa instalado e executando. Isso abre uma possibilidade de transitar entre os sites que estão dentro de um mesmo servidor, se ao menos um destes sites tiver uma falha que exponha isso.

### Subdomínios

Subdomínios são tipos diferentes de endereços do site que nos levam para um outro site da mesma organização ou a um serviço do mesmo.

Pelo Kali, usando a ferramenta pre-instalada "Kockpy", nos possibilita testar a quantidade de subdomínios de um site por meio do bruteforce usando uma wordlist (por exemplo).

Scan simples de um subdomínio:
```bash
knockpy --domain site.com --recon
```

### Diretórios e Arquivos

Usaremos a ferramenta "Dirb" | https://www.kali.org/tools/dirb/

Com esse simples comando o Dirb usa sua wordlist padrão para o scam de arquivos e analisa o site:
```bash
dirb site.com
```

O scan do dirb tenta acessar e analisar todos os arquivos/diretórios que compõe o site. Use a flag -r antes da URL desejada para analisar apenas um único local.

! teste (intitle:"index of" file: config.inc)