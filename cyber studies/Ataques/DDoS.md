# Dos/DDoS - Negação de Serviço

Neste ataque são utilizadas formas de sobrecarregar servidores, sites e redes de computadores até o ponto da comunicação 
não aguentar a quantidade de fluxo, deixando o sistema inoperante. Neste ataque são utilizadas ferramentas que geram requisiões
de forma artificial ou utilizando um conjunto de hosts que enviam milhões de requisição a um alvo de maneira conjunta.

A diferença entre DoS e DDoS é o D de *Distributed*. No DoS um único host é utilizado para gerar o congestionamento da rede.
Diferente do modo distribuído, utilizando uma rede de botnets para gerar as requisições e interromper a comunicação. 

Ao decorrer da leitura você entendera do porque este ataque existe e porque os sistemas de informação são vulneráveis a ele.

## Como é Elaborado

Se tratando de DDoS, as formas conhecidas para iniciar o ataque é por meio da criação da rede botnet infectando outros hosts de 
forma silenciosa podendo ser operados pelos hackers remotamente os usando para gerar o tráfego falso. Todo tipo de aparelho que 
conecta a uma rede de computadores pode ser usado como botnet (celulares, computadores, aparelhos IoT, etc). A outra forma é 
alugando botnets fornecidas em ambientes da darkweb, como uma proposta de DoS as a Service.

Ao efetuar a interrupção de serviço os hackers podem escolher em enviar o máximo de requisições possíveis pelos hosts da botnet
ou enviar pequenas fatias aumentando o tráfego gradualmente até o ponto da desestabilizar a rede. 

No DDoS pode ser pelos APT's(Ameaça Avançada Persistente) como maneira de desviar a atenção de equipes de segurança enquanto
efetuam algo maior dentro do seu esquema de ataque, como roubar os dados da organização alvo.

## Tipos de DoS

- Ataques de protocolo: mira nos protocolos presente nas camadas 3 e 4 (Rede e Transporte) do modelo OSI, as quais trabalham com
o estabeleciomento de comunicação entre hosts e envio de pacotes por meio de dispositivos de rede que trabalham nestas camadas.
A sobrecarga é feita pelo envio de SYN Packages, enviando handshakes sem a confirmação de uma comunicação ou por envio de pacotes
ICMP. Pelo ICMP é feito envio de uma resposta eco que entra em repetição pelos IP mascarado da botnet de forma contínua ao alvo.
Qualquer outro protocolo de rede que consiste no envio de pacotes de requisição pode ser usado.

- Ataques volumétricos: essa é forma de ataque onde a maior quantidade de pacotes possível é enviada ao alvo por meio do envio de
pacotes UDP ou ICMP falsos. O ICMP nesta situação é o mais comum a ser utilizado, fazendo o sistema alvo sendo obrigado a responder
as requisições enviadas, sendo sufucado e terminando inoperante.

- Ataques multivetoriais: o escopo do ataque abrange a combinação de outros visando prejudicar um sistema de diferentes formas e
com mais impacto.

Podemos ver a facilidade de como são efetuados os ataques.

---
### Referências:
[IBM - O que é um ataque de DDoS?](https://www.ibm.com/br-pt/think/topics/ddos)
[Akamai - What is DDoS?](https://www.akamai.com/pt/glossary/what-is-ddos)
