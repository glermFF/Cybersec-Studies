# Wireshark: guia prático

## O que é o Wireshark

O Wireshark é um analisador de protocolos open source que captura e mostra o tráfego de rede pacote a pacote, com decodificação de centenas de protocolos. É a ferramenta de referência para diagnosticar problemas de comunicação: mostra o que realmente passou pela rede, em vez de suposições.

- Software livre (licença GPL), mantido por uma comunidade grande e pela Wireshark Foundation, com atualizações frequentes e novos decodificadores de protocolos.
- Multiplataforma: Windows, Linux e macOS.
- É também uma das ferramentas mais usadas para aprender redes, porque mostra na prática o que os livros descrevem (handshake TCP, DNS, DHCP, TLS).

### Funcionalidades

- **Capturar** tráfego ao vivo de uma interface (cabeada ou Wi-Fi) ou **abrir** capturas salvas (`.pcap`/`.pcapng`).
- **Decodificar** os protocolos camada por camada, com cada campo explicado.
- **Filtrar** o que interessa e **colorir** pacotes por regras.
- Gerar **estatísticas** (hierarquia de protocolos, conversas, gráficos de E/S, tempos de resposta).
- **Reconstruir** conversas (Follow Stream) e **extrair arquivos** de protocolos sem criptografia.
- Analisar **chamadas VoIP** (jitter, perda) e fluxos RTP.
- **Decifrar TLS do seu próprio navegador** usando o arquivo de chaves de sessão (`SSLKEYLOGFILE`). Serve só para o tráfego da sua própria máquina, em estudo e testes.

### Áreas e profissionais que costumam usar

| Área | Quem costuma usar | Para quê |
|---|---|---|
| Redes e infraestrutura | Técnicos, analistas e engenheiros de redes, equipes de NOC | Latência, perda de pacotes, MTU, loops, falhas de roteamento e de DNS |
| Provedores de internet e telecom | Suporte técnico N2/N3, engenheiros de testes, técnicos de campo | Falhas de PPPoE e DHCP, IPTV, VoIP, validação de equipamentos (CPE, ONU, roteadores) |
| Segurança da informação | Analistas de SOC, resposta a incidentes, perícia forense digital, pentesters | Investigar tráfego suspeito, beaconing, exfiltração de dados, validar alertas de IDS/IPS |
| Desenvolvimento e operações | Desenvolvedores, QA, SRE e DevOps | Depurar a comunicação entre cliente e servidor, handshakes TLS, tempos de resposta de APIs |
| Voz e vídeo | Engenheiros de telefonia e colaboração | SIP/RTP, jitter, perda e qualidade de chamadas |
| IoT e automação industrial | Engenheiros de automação e sistemas embarcados | Protocolos como MQTT e Modbus, comportamento de dispositivos |
| Fabricantes de equipamentos | Engenheiros de firmware e QA de hardware | Verificar se o equipamento implementa o protocolo corretamente |
| Ensino e pesquisa | Estudantes, professores, pesquisadores | Aprender protocolos, preparar-se para certificações de redes e segurança, estudos de tráfego |

### Atividades recomendadas para praticar

Em ordem crescente de dificuldade. Faça em **ambiente próprio** (seu computador e sua rede):

1. **Navegar e observar:** capture enquanto abre um site e identifique a sequência DNS → handshake TCP → TLS → dados.
2. **Handshake TCP:** filtre `tcp.flags.syn == 1` e acompanhe SYN, SYN/ACK e ACK, depois o encerramento (FIN/RST).
3. **Ping e traceroute:** veja ICMP e o TTL diminuindo a cada salto.
4. **DNS:** consulte nomes com `nslookup` ou `dig` e compare pergunta, resposta e tempo (`dns.time`).
5. **DHCP:** libere e renove o IP da máquina e identifique as quatro mensagens (Discover, Offer, Request, Ack).
6. **ARP:** limpe o cache ARP e veja quem pergunta por quem.
7. **HTTP x HTTPS em laboratório:** suba um servidor local simples (`python3 -m http.server`), acesse com `curl` e use *Follow TCP Stream* para ver o texto claro. Depois compare com uma conexão HTTPS.
8. **Decifrar o seu próprio TLS:** configure `SSLKEYLOGFILE` no navegador e carregue o arquivo em *Preferences → Protocols → TLS*.
9. **Simular problemas (apenas em laboratório):** em Linux, use `tc netem` para adicionar perda ou atraso e observe retransmissões e RTT no Wireshark.
10. **Analisar capturas públicas:** use as *Sample Captures* da wiki do Wireshark e exercícios públicos de análise de pcap (como os de Malware-Traffic-Analysis.net e plataformas de treino em segurança). Em capturas de malware, analise os pacotes, mas nunca execute arquivos extraídos fora de um ambiente isolado.

### Boas práticas de uso profissional

- Crie **perfis de configuração** (*Edit → Configuration Profiles*), por exemplo "Suporte", "Wi-Fi" e "Segurança", cada um com suas colunas, cores e botões de filtro.
- Salve filtros frequentes como **botões** na barra de filtros.
- Para capturas longas, use **ring buffer** para não encher o disco:

```bash
dumpcap -i wlan0 -b duration:3600 -b files:24 -w captura.pcapng
```

- Use **comentários em pacotes** (*Edit → Packet Comment*) para registrar o que cada achado significa.
- Lembre-se dos limites: o Wireshark não é um monitor contínuo nem gera alertas, e mostra apenas o que passa no ponto de captura (veja a seção 9).

### Ferramentas que acompanham a instalação

Utilitários incluídos:

| Ferramenta | Função |
|---|---|
| `wireshark` | Interface gráfica |
| `tshark` | Versão em linha de comando (automação, extração) |
| `dumpcap` | Captura leve (usada pelo Wireshark) |
| `capinfos` | Resumo de um arquivo de captura |
| `editcap` | Cortar, filtrar e converter capturas |
| `mergecap` | Juntar várias capturas |

## Onde e como capturar

O que você consegue ver depende do **ponto de captura**:

- **Rede cabeada com switch:** só o seu tráfego, mais broadcast/multicast e algum unicast desconhecido.
- **Wi-Fi em modo normal (managed):** seu tráfego e broadcast/multicast. O modo promíscuo não revela o tráfego de outros clientes.
- **Wi-Fi em modo monitor:** quadros 802.11 do canal (beacons, probes, gerenciamento). O conteúdo dos dados de outros clientes está cifrado (WPA2/WPA3).
- **Visão da rede toda (com autorização):** porta espelhada (SPAN), **TAP**, captura no próprio AP/roteador (`tcpdump` em OpenWrt/Linux), dados de fluxo (NetFlow/IPFIX/sFlow) e telemetria da controladora.

Capturas remotas simples:

```bash
# captura no roteador/CPE Linux e abre no Wireshark depois
tcpdump -i eth0 -s 96 -w captura.pcap

# só cabeçalhos (menos dados pessoais): -s 96
# filtro de captura: só um host e uma porta
tcpdump -i eth0 host 192.168.0.10 and port 53 -w dns.pcap
```

### Filtro de captura x filtro de exibição

| Característica | Filtro de captura (BPF) | Filtro de exibição |
|---|---|---|
| Quando | **Antes** de capturar | **Depois**, sobre o que já foi capturado |
| Sintaxe | `host 10.0.0.1 and port 53` | `ip.addr == 10.0.0.1 && dns` |
| Efeito | Descarta o que não casa | Só esconde, não apaga |

Na dúvida, capture mais e filtre depois (respeitando privacidade).

## Interface: o essencial

- **Lista de pacotes / detalhes / bytes:** os três painéis. Clique em um pacote e expanda as camadas (Ethernet → IP → TCP/UDP → aplicação).
- **Colunas úteis:** adicione *Delta time*, `tcp.stream`, `dns.qry.name`, `tls.handshake.extensions_server_name` (botão direito num campo → *Apply as Column*).
- **Coloring rules:** vermelho/preto costuma indicar problemas (retransmissão, RST).
- **Name Resolution** (*View → Name Resolution*): resolve MAC para fabricante e IP para nome.
- **Follow Stream** (botão direito → *Follow → TCP/UDP/TLS Stream*): reconstrói a conversa.
- **Export Objects** (*File → Export Objects → HTTP*): extrai arquivos de HTTP em texto claro.

### Menus de estatística (comece por eles)

| Menu | Para quê |
|---|---|
| *Statistics → Capture File Properties* | Duração, pacotes, interface |
| *Statistics → Protocol Hierarchy* | O que domina o tráfego |
| *Statistics → Conversations* / *Endpoints* | Quem fala com quem, quanto |
| *Statistics → I/O Graph* | Picos ao longo do tempo |
| *Statistics → Resolved Addresses* | Nomes resolvidos |
| *Analyze → Expert Information* | Retransmissões, resets, anomalias |
| *Telephony → VoIP Calls / RTP* | Qualidade de chamadas |

## Filtros de exibição úteis

### Gerais

```
ip.addr == 192.168.0.10
tcp.port == 443
eth.addr == aa:bb:cc:dd:ee:ff
!(arp || dns || icmp)
frame.time >= "2026-10-23 10:00:00"
```

### Por protocolo

| Objetivo | Filtro |
|---|---|
| DNS | `dns` |
| Consultas DNS lentas | `dns.time > 0.5` |
| DHCP | `dhcp` (ou `bootp`) |
| ARP | `arp` |
| PPPoE | `pppoed \|\| pppoes \|\| ppp` |
| TLS (início da conexão) | `tls.handshake.type == 1` |
| Nome do site (SNI) | `tls.handshake.extensions_server_name` |
| TLS antigo | `tls.handshake.version < 0x0303` |
| QUIC | `quic` |
| HTTP | `http` |
| Descoberta local | `mdns \|\| llmnr \|\| nbns \|\| ssdp` |
| IPv6 (NDP/RA) | `icmpv6` |
| IGMP (multicast/IPTV) | `igmp` |
| STP | `stp` / `stp.flags.tcn` |
| Broadcast | `eth.dst == ff:ff:ff:ff:ff:ff` |
| Wi-Fi: retransmissões (modo monitor) | `wlan.fc.retry == 1` |
| Wi-Fi: beacons (modo monitor) | `wlan.fc.type_subtype == 0x08` |

### Problemas de desempenho (TCP)

```
tcp.analysis.flags
tcp.analysis.retransmission || tcp.analysis.fast_retransmission
tcp.analysis.zero_window
tcp.analysis.duplicate_ack
tcp.flags.reset == 1
```

### Texto claro e autenticação

```
http || ftp || telnet || smtp || pop || imap || snmp || ldap
http.authorization
http.request.method == "POST"
```

## Cenários de suporte ao cliente (provedor)

| # | Sintoma | Onde capturar | O que observar |
|---|---|---|---|
| 1 | Não conecta (PPPoE) | WAN do CPE | PADI → PADO → PADR → PADS; depois PAP/CHAP. Sem PADO = problema do lado da rede; `Authenticate-Nak` = credencial/cadastro |
| 2 | Sem IP ou IP errado | WAN/LAN do CPE | Discover/Offer/Request/Ack. Sem Offer = servidor ou relay (option 82) não responde; vários Offers = DHCP duplicado |
| 3 | "Internet lenta" com link ok | Cliente e CPE | Retransmissões, janela zero, RTT (*TCP Stream Graphs → Round Trip Time*). Onde a perda começa indica Wi-Fi, ONU ou rede externa |
| 4 | Alguns sites abrem, outros não | WAN do CPE | ICMP `type 3 code 4` (fragmentation needed), MSS inconsistente. Clássico em PPPoE (MTU 1492) com ICMP bloqueado |
| 5 | Demora para abrir páginas | Cliente | `dns.time`, ausência de resposta, resolver lento |
| 6 | IPTV travando | LAN do CPE | `igmp` (join chegando?), perda no multicast |
| 7 | Chamadas VoIP ruins | LAN/WAN | *Telephony → RTP Stream Analysis*: jitter e perda por chamada |

## Cenários de teste de equipamentos

- **Throughput e carga:** gere tráfego com `iperf3`, capture dos dois lados e compare perda, reordenação e atraso. Use `mtr` para ver o caminho.
- **Casos de borda e resiliência:** monte pacotes específicos com **Scapy** (DHCP, ARP, ICMP, VLAN) e observe a reação do equipamento.
- **Loop e broadcast storm:** *Statistics → Conversations* mostra um MAC dominando; observe `stp.flags.tcn` em excesso.
- **VLAN/QinQ:** filtro `vlan` e `vlan.id == X` para validar tagging.
- **Wi-Fi do cliente (modo monitor):** retransmissões e clientes presos em APs distantes (`wlan.fc.retry == 1`).

Ao testar: anote versão do firmware, topologia, configuração, horário e hash do pcap. Sem isso, o teste não é repetível.

## `tshark`: automação e extração

```bash
CAP=captura.pcapng
top() { sort | uniq -c | sort -rn | head -20; }   # contagem dos mais frequentes

capinfos $CAP                                      # resumo
tshark -r $CAP -q -z io,phs                        # protocolos
tshark -r $CAP -q -z conv,ip                       # conversas

# domínios mais consultados
tshark -r $CAP -Y "dns.flags.response==0" -T fields -e dns.qry.name | top

# sites acessados via TLS (SNI)
tshark -r $CAP -Y "tls.handshake.type==1" -T fields -e tls.handshake.extensions_server_name | top

# exportar para CSV (para pandas)
tshark -r $CAP -T fields -E header=y -E separator=, \
  -e frame.time_epoch -e ip.src -e ip.dst -e frame.len > pacotes.csv
```

Cortar e juntar capturas:

```bash
editcap -A "2026-10-23 10:00:00" -B "2026-10-23 10:30:00" in.pcapng recorte.pcapng
mergecap -w tudo.pcapng parte1.pcapng parte2.pcapng
```

## Roteiro de prática: captura da universidade

Para esta prática, siga em paralelo o roteiro do `zeek-guia-pratico.md`. Use o Wireshark para **aprofundar** o que o Zeek apontar.

1. Abra o arquivo e veja *Capture File Properties*, *Protocol Hierarchy*, *Conversations* e *I/O Graph*.
2. Identifique seu IP/MAC e use `ip.addr == SEU_IP` para separar o seu tráfego do broadcast/multicast.
3. Mapeie dispositivos: `arp`, `dhcp`, `mdns || llmnr || nbns || ssdp`, com resolução de MAC para fabricante ligada.
4. Liste domínios e SNI (colunas `dns.qry.name` e `tls.handshake.extensions_server_name`).
5. Procure texto claro: `http || ftp || telnet || smtp || pop || imap || snmp || ldap`.
6. Revise *Expert Information* e `tcp.analysis.flags`.
7. Para cada conexão curiosa do Zeek, filtre IP e porta e use *Follow Stream*.
8. Anote achados com filtro, horário e print (sem dados de terceiros).

## Limites e cuidados

- Hoje a maior parte do tráfego usa **TLS/QUIC**: você analisa o *comportamento* (handshake, tempos, retransmissões, SNI), não o conteúdo.
- Em Wi-Fi no modo normal, a captura **não representa a rede inteira**.
- Capturas podem conter **dados pessoais** (LGPD). Capture só o necessário (`-s 96`, filtros de captura), apague após o atendimento e, quando possível, tenha a autorização do cliente ou da instituição.
- Capturar ou decifrar tráfego de terceiros sem autorização pode violar a política da instituição e a legislação.
- Não compartilhe pcaps ou prints sem anonimizar IPs, MACs, nomes e credenciais.
