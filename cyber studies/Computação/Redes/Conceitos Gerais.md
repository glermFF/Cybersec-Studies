Ao se referir a uma rede de computadores, estamos falando de um sistema de comunicação complexo que integra equipamentos, sistemas para estabelecer a comunicação e protocolos que seriam uma linguagem utilizada para os computadores entenderem o que é comunicado.

Cada protocolo tem uma função específica dependendo do tipo de dado quer for compartilhar, exemplo: usar bluetooth enviar um arquivo para um dispositivo sem conexão wifi.

Os seguintes tópicos serão abordados para entendimento das redes de computadores:

- Formas de Comunicação
	- [[Redes Wireless]] 
	- Topologias
	- [[Protocolos de Rede]]
- Modelo OSI
	- Camadas
		- Aplicação 
		- Sessão
		- Rede
		- Enlace
		- Transporte
		- Física
- Modelo TCP
	- Camadas
		- Aplicação
		- Rede
		- Enlace
		- Física

---
## Formas de Comunicação

> Cada equipamento tem uma devida função em uma rede onde uns apenas propagam o canal de comunicação, outros conseguem criar a comunicação e enviar dados e há aqueles que fazem tudo de forma simultânea. Essas são os 3 tipos de comunicação:

**Simplex:** Apenas propagam a informação e criar uma área de comunicação para outros dispositivos. Equipamentos: Atenas de operadoras telefônicas, repetidores de sinal.

**Duplex:** Conseguem estabelecer um canal de comunicação entre 2+ aparelhos e enviar dados entre eles. O ponto aqui é que quando um equipamento está transmitindo o dado, o outro deve esperar que a transmissão termine e depois conseguir responder. Equipamentos: walkie-tolkie

**Full-Duplex:** Os equipamentos podem enviar dados ao mesmo tempo que outros estão enviando sem que haja problemas de interrupção ou perda de dados (REVISAR)

## Topologias

> Sistemas de Informação tem em sua maioria dispositivos Full-Duplex (PC's) mas os outros modos de comunicação também são presentes em Impressoras e repetidores de sinal para melhor alcance. Tendo este conhecimento, é analisado e implementado uma topologia física de rede que melhor encaixa no cenário onde de modo que os computadores possam se comunicar da maneira que mais se encaixa ali (REVISAR).
### Topologia de Barramento

> Os dados são transmitidos por uma única via bidirecional onde os computadores estão conectados. Pelo fato das informações trafegarem o tempo todo e não há um administrador de redes que direcione de maneira que evite problemas, colisões de dados são comuns de ocorrer. Não apenas isto, como o problema de todos os computadores receber dados que não foram direcionados para ele.

### Topologia Estrela

> Tipo mais comum que podemos encontrar. Esta topologia possui um aparelho usado como direcionador dos dados que passam pela rede, um switch ou roteador. Por ela, todos os computadores podem se comunicar entre si e com garantia que os dados enviados cheguem ao destino sem que seja espalhado para toda a rede.

### Topologia P2P (Point-to-Point)

> Um dispositivo A conecta diretamente em um dispositivo B onde um lado tem função de enviar dados e outro tem a função de receber os dados. Sua conexão é bem eficiente por haver uma única via onde trafegam os dados de 2 dispositivos. Por meio do P2P, podemos manter uma rede descentralizada onde se houver problema em dos equipamentos ela ainda continua a funcionar por não depender de um meio principal que conecte todos os dispositivos. Outra vantagem é a segurança. Dificilmente ocorrerá uma interrupção ou desvio de dados por um interceptador. 
### Topologia em Anel

> A topologia anel conecta os computadores de forma que a informação trafegue de modo circular, passando em cada dispositivo por uma via unidirecional. O problema aqui já observador de cara é em caso do primeiro computador precisar enviar ao vizinho que não segue a via de dados, fazendo uma volta até o final do anel.

### Topologia em Árvore

> Implementada com uma central de transmissão de dados que funciona na forma de camadas em hierarquia. Para o acesso de cada camada há um hub/switch estão ligados diretamente a central de compartilhamento. Mesmo trazendo a vantagem de um sistema seguro para o fluxo de informações sensíveis, há o grave problema de se cortada a comunicação com a central, toda a comunicação é interrompida.

---
## Modelos OSI

>O modelo OSI foi criado como um guia mostrando uma maneira de como implementar um modelo de comunicação para sistemas de informação apresentando 7 camadas onde cada uma delas utiliza determinado equipamento, software, protocolo para o melhor funcionamento da transmissão dos dados. Este modelo foi usado para a definição do modelo TCP/IP. Amplamente usado atualmente.

## Camadas

### Aplicação

>Camada que está mais próxima do usuário, a camada de Aplicação é responsável em oferecer o devido serviço de acordo com a configuração de um servidor/serviço dentro da rede. É nela que encontramos certos protocolos de rede (ex: FTP, DNS, Telnet, SMTP, etc) responsáveis no envio de dados entre 2 dispositivos.

**Componentes:** sistemas Web, sistemas de email
### Apresentação

>Referida como tradução, essa camada tem como função comprimir e codificar/decodificar da informação que chegam para o receptor ou que é enviada pelo transmissor. Essa tradução dos dados é feito pelo protocolo SSL(Secure Socket Layer) que providência segurança na transmissão de dados por meio de técnicas de criptografia.

**Componentes:** 

### Física

> Por meio desta camada estão localizados os componentes de hardware responsáveis em conectar dispositivos na rede, modular o sinal e guiar os bits de dados ao destino. 
> A transmissão dos dados pode ser feito por meio guiado, usando uma estrutura de cabeamento de cabos par-trançado que interligam os dispositivos em suas placas de rede ou meio não guiado, utilizando equipamentos que emitem sinais de rádio, o famoso wi-fi.

> O meio guiado não utiliza apenas os cabos par-trançado, eles são o tipo mais popular encontrado em redes domésticos de organizacionais. Há os cabos de fibra óptica que conseguem transmitir uma quantidade de dados por segundo maior e percorrerem maiores distâncias sem que haja perda de pacotes, comparados aos cabos convencionais. Tudo isto feito por meio de emissão de bits de sinais de luz/laser.
> 
> > Cabos de fibra óptica conseguem transmitir dados de até 10Gbps/s na rede.

> A tempos passados, o tipo de cabo comum era o coaxial, muito utilizado em aparelhos de transmissão de TV. Cabos esses com uma estrutura rígida, envolto numa blindagem, uma malha de fibra e uma camada de plástico. Os bits de informação eram transmitidos por meio de sinais elétricos passando por uma camada de cobre.
> 
> > Cabos coaxiais mesmo não sendo muito utilizados em redes domésticas possui uma transmissão eficiente de dados, até 1GHz. Conseguem percorrer maiores distâncias em comparação com os cabos par-trançado

> **Compõe a camada física:** modem, cabos de rede (par-trançado ou fibra óptica), cabos coaxial, repetidores, hub

---
