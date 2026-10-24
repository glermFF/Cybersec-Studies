# Zeek: guia prático e roteiro de prática

## Conhecendo Zeek

O Zeek (antigo Bro) é um framework open source de monitoramento e análise de tráfego de rede. Ele não é só um IDS: em vez de mostrar pacote por pacote, ele interpreta o tráfego e escreve logs estruturados por protocolo (conexões, DNS, TLS, HTTP, DHCP etc.).

- Lê **arquivos `.pcap/.pcapng`** (análise offline) ou **interfaces ao vivo**.
- Gera logs em texto (TSV) ou **JSON**, ideais para `pandas`, Grafana, ELK.
- Tem uma linguagem de scripts própria para criar detecções e logs customizados.
- Funciona bem para **panorama e estatística**: quem falou com quem, quanto, por quanto tempo, com qual protocolo.

### Origem

- Criado por **Vern Paxson** nos anos 1990, no Lawrence Berkeley National Laboratory (EUA), com o nome **Bro**. Foi renomeado para **Zeek** em 2018.
- Software livre (licença BSD), mantido por uma comunidade que reúne instituições de pesquisa, universidades e empresas.
- Muito usado em redes de universidades, laboratórios e grandes organizações para **monitorar a segurança da rede**.

### Qual é o intuito do Zeek

O Zeek foi pensado para **monitoramento de segurança de rede (NSM, *Network Security Monitoring*)**. A ideia central é dar **visibilidade e contexto**: registrar de forma organizada *o que aconteceu* na rede, para que uma pessoa (ou outro sistema) analise depois.

- **Não é um IPS:** ele observa uma cópia do tráfego (SPAN/TAP ou arquivo de captura) e **não bloqueia** nada.
- **Registra transações, não pacotes:** em vez de milhões de pacotes, você recebe "o host A consultou o domínio X às 10:32, depois abriu uma conexão TLS com o servidor Y por 4 minutos".
- **Não depende só de assinaturas:** ele descreve o comportamento e deixa a decisão de "isso é suspeito?" para você, para scripts ou para outras ferramentas.
- **Identifica protocolos pelo conteúdo, não só pela porta:** um serviço rodando em porta incomum ainda pode ser reconhecido.

### Como ele funciona (visão simplificada)

```
Tráfego (interface ou .pcap)
        ↓
Motor de eventos + analisadores de protocolo (DNS, TLS, HTTP, DHCP...)
        ↓
Scripts (lógica de detecção e de registro, personalizáveis)
        ↓
Logs estruturados (conn.log, dns.log, ssl.log...)
```

Os logs são **ligados entre si pelo `uid`** de cada conexão, o que permite reconstruir o que aconteceu em uma comunicação a partir de vários logs.

### Principais recursos

- **Logs prontos** para os protocolos mais comuns, em TSV ou JSON.
- **Scripts em linguagem própria** para criar detecções, campos extras e logs novos.
- **Gerenciador de pacotes (`zkg`)** para instalar scripts da comunidade.
- **Extração e hash de arquivos** transferidos em protocolos sem criptografia.
- **Framework de inteligência de ameaças:** compara IPs, domínios e hashes do tráfego com listas de indicadores.
- **Execução em cluster** para redes de alto volume.
- **Modo ao vivo** (`zeek -i interface`) e modo offline (`zeek -r arquivo.pcap`).

### Onde utilizar

| Cenário | Exemplo de uso | Por que o Zeek ajuda |
|---|---|---|
| Segurança e *threat hunting* | Buscar domínios suspeitos, conexões muito longas, comunicações periódicas (*beaconing*) | Logs completos e pesquisáveis, fáceis de enviar a um SIEM |
| Resposta a incidentes e perícia | Processar o pcap de um incidente e montar uma linha do tempo | Gera um resumo estruturado em segundos, mesmo de capturas grandes |
| Monitoramento de rede (campus, empresa, provedor) | Inventário de hosts e serviços, uso de TLS antigo, DNS anômalo | Visão contínua a partir de espelhamento de porta ou TAP |
| Auditoria e conformidade | Evidenciar protocolos em texto claro e certificados expirados | Registros objetivos e repetíveis |
| Pesquisa e ensino | Estudar datasets de tráfego, trabalhos acadêmicos, laboratórios | Padroniza a análise e facilita a reprodução |
| Ciência de dados | Extrair atributos de tráfego para estatísticas ou modelos | Logs já prontos para `pandas` |
| Testes de equipamentos em laboratório | Processar várias capturas de teste e comparar resultados entre configurações ou firmwares | Mesmo conjunto de logs para todas as capturas, comparável |

### Quando outra ferramenta é melhor

- **Depurar um problema pontual pacote a pacote:** use o **Wireshark**.
- **Bloquear tráfego malicioso:** o Zeek não bloqueia; use firewall ou um IPS (por exemplo, Suricata em modo inline).
- **Ver o conteúdo de tráfego cifrado:** o Zeek também não decifra TLS; ele registra o comportamento (SNI, versão, tempos).
- **Painéis em tempo real prontos:** o **ntopng** ou o Grafana apresentam isso de forma mais direta.

### Integrações comuns

- **Suricata + Zeek:** o Suricata alerta por assinatura e o Zeek fornece o contexto da conexão.
- **ELK/OpenSearch, Splunk, Grafana:** para armazenar, pesquisar e visualizar os logs.
- **Security Onion:** distribuição que já vem com Zeek, Suricata e interface de análise.
- **RITA:** analisa logs do Zeek em busca de comportamento periódico típico de *beaconing*.

### Limites e cuidados

- Tem uma **curva de aprendizado** (formato dos logs e, para ir além, a linguagem de scripts).
- Só enxerga **o que passa no ponto de captura**: em Wi-Fi no modo normal, é o seu tráfego e o broadcast/multicast.
- Os logs de DNS e TLS (domínios, SNI) podem revelar hábitos de uso: tratam-se de **dados pessoais** (LGPD). Guarde com cuidado e anonimize antes de compartilhar.

### Zeek x Wireshark

| | Wireshark | Zeek |
|---|---|---|
| Foco | Detalhe de pacote a pacote | Visão de conexões e protocolos |
| Saída | Interface gráfica | Logs estruturados |
| Melhor para | Investigar um problema específico | Entender o todo e automatizar |
| Escala | Capturas pequenas/médias | Capturas grandes e monitoramento contínuo |

Uso combinado: **Zeek mostra onde olhar, Wireshark mostra o que aconteceu ali.**

## Instalação

Opções comuns (confira a documentação oficial para a sua distribuição):

- Pacote da sua distribuição ou repositório oficial do Zeek.
- **Docker** (não precisa instalar nada no sistema):

```bash
docker run --rm -v "$PWD":/data -w /data zeek/zeek zeek -C -r captura.pcapng
```

Verifique com:

```bash
zeek --version
zeek-cut --help
```

## Primeiros passos

```bash
mkdir analise && cd analise
cp ../captura.pcapng .
zeek -C -r captura.pcapng
ls *.log
```

- `-r` lê o arquivo de captura.
- `-C` ignora a validação de checksum (capturas feitas na própria placa costumam ter checksum "errado" por offload da placa de rede).
- O Zeek cria os logs **no diretório atual**. Por isso, rode sempre dentro de uma pasta própria.

### Logs mais úteis

| Log | O que contém |
|---|---|
| `conn.log` | Todas as conexões: origem, destino, porta, protocolo, duração, bytes, estado |
| `dns.log` | Consultas e respostas DNS (inclui mDNS/LLMNR, normalmente nas portas 5353/5355) |
| `ssl.log` | Versão TLS, SNI (`server_name`), cifra, se a sessão foi estabelecida |
| `x509.log` | Certificados (em TLS 1.3 o certificado vai cifrado, então pode faltar) |
| `http.log` | Requisições em texto claro: host, URI, método, user-agent |
| `dhcp.log` | MAC, hostname, IP atribuído, lease |
| `weird.log` | Comportamentos anômalos ou protocolos fora do padrão |
| `notice.log` | Alertas gerados por scripts |
| `files.log` | Arquivos transferidos (quando identificáveis) |

### `zeek-cut`: escolher colunas

```bash
# colunas específicas
cat conn.log | zeek-cut id.orig_h id.resp_h id.resp_p proto service

# horário legível
cat conn.log | zeek-cut -d ts id.orig_h id.resp_h id.resp_p

# ver os nomes das colunas de um log
head -n 10 conn.log | grep '#fields'
```

Atenção: `zeek-cut` funciona com logs em formato texto (TSV). Para JSON use `jq` ou pandas.

### Logs em JSON (para pandas)

```bash
mkdir json && cd json
zeek -C -r ../captura.pcapng LogAscii::use_json=T
```

```python
import pandas as pd
conn = pd.read_json("conn.log", lines=True)
dns  = pd.read_json("dns.log",  lines=True)
ssl  = pd.read_json("ssl.log",  lines=True)
```

### O campo `uid`

Cada conexão tem um `uid` único, que aparece em `conn.log`, `dns.log`, `ssl.log` etc. Ele permite **seguir uma conexão entre os logs**. Para abrir a mesma conexão no Wireshark, use IP e portas do `conn.log`:

```
ip.addr == 192.168.0.10 && tcp.port == 443
```

## Roteiro de prática: captura da universidade

**Contexto:** captura feita no computador, na interface Wi-Fi em modo normal, em um AP da universidade. Espera-se ver principalmente o seu tráfego e o broadcast/multicast da rede.

**Regra de ouro:** se aparecer tráfego de terceiros, não compartilhe, não tente decifrar e anonimize antes de mostrar prints.

### Etapa 0: Preparação

Objetivo: deixar a análise reproduzível.

```bash
mkdir -p ~/projeto-captura/{pcap,zeek-tsv,zeek-json,notas}
cp captura.pcapng ~/projeto-captura/pcap/
sha256sum ~/projeto-captura/pcap/captura.pcapng > ~/projeto-captura/notas/hash.txt
capinfos ~/projeto-captura/pcap/captura.pcapng
```

Anote: duração da captura, número de pacotes, tamanho, data e hora, nome do AP, o IP e o MAC do seu computador.

### Etapa 1: Gerar os logs

```bash
cd ~/projeto-captura/zeek-tsv
zeek -C -r ../pcap/captura.pcapng

cd ../zeek-json
zeek -C -r ../pcap/captura.pcapng LogAscii::use_json=T
```

Pergunta: quais logs foram criados? Os que não aparecem indicam protocolos ausentes na captura.

### Etapa 2: Visão geral das conexões

```bash
cd ~/projeto-captura/zeek-tsv

# protocolos / serviços identificados
cat conn.log | zeek-cut service | sort | uniq -c | sort -rn

# portas de destino mais usadas
cat conn.log | zeek-cut proto id.resp_p | sort | uniq -c | sort -rn | head -20

# destinos que mais receberam/enviaram bytes
cat conn.log | zeek-cut id.resp_h orig_bytes resp_bytes \
  | awk '{b[$1]+=$2+$3} END{for(i in b) print b[i], i}' | sort -rn | head -15

# estados de conexão (S0 = sem resposta, REJ = rejeitada)
cat conn.log | zeek-cut conn_state | sort | uniq -c | sort -rn
```

Perguntas:
1. Quais são os 5 destinos com mais tráfego? Quem são (use `nslookup`/`whois` ou o DNS do próprio log)?
2. Há muitas conexões `S0` ou `REJ`? Isso indica o quê?
3. Qual proporção do tráfego é broadcast/multicast?

### Etapa 3: DNS

```bash
# domínios mais consultados
cat dns.log | zeek-cut query | sort | uniq -c | sort -rn | head -30

# tipos de consulta
cat dns.log | zeek-cut qtype_name | sort | uniq -c | sort -rn

# respostas com erro (NXDOMAIN etc.)
cat dns.log | zeek-cut query rcode_name | awk '$2!="NOERROR" && $2!="-"' | sort | uniq -c | sort -rn | head

# consultas mDNS/LLMNR (descoberta local)
cat dns.log | zeek-cut id.resp_p query | awk '$1==5353 || $1==5355' | sort | uniq -c | sort -rn | head -20
```

Perguntas:
1. Quais serviços de fundo (atualização, telemetria, nuvem) aparecem sem você ter aberto nada?
2. Há muitos NXDOMAIN? Quais domínios?
3. Quais nomes locais (`.local`) aparecem? Que dispositivos eles sugerem?

### Etapa 4: Documentar

Para cada achado, registre em `notas/`:

- **O que** foi encontrado e **onde** (log, filtro, horário).
- **Evidência** (comando usado, print, trecho do log).
- **Interpretação** (o que significa) e **grau de certeza**.
- **Próximo passo** (investigar no Wireshark, repetir captura etc.).

### Checklist final

- [ ] Hash e metadados da captura registrados
- [ ] Logs do Zeek gerados em TSV e JSON
- [ ] Visão geral das conexões levantada (serviços, portas, destinos, estados)
- [ ] Domínios mais consultados e respostas com erro identificados
- [ ] Achados documentados com evidência

## Sugestões de uso

Depois de concluir as etapas 0 a 4, estes são temas para explorar com a mesma captura, na ordem que fizer mais sentido para o seu projeto. Os comandos assumem que você está na pasta `zeek-tsv` (ou `zeek-json`, no caso do pandas).

### TLS e criptografia

```bash
cd ~/projeto-captura/zeek-tsv

# sites acessados (SNI)
cat ssl.log | zeek-cut server_name | sort | uniq -c | sort -rn | head -30

# versões de TLS em uso
cat ssl.log | zeek-cut version | sort | uniq -c | sort -rn

# cifras usadas
cat ssl.log | zeek-cut cipher | sort | uniq -c | sort -rn | head

# certificados (quando visíveis): emissor e validade
cat x509.log | zeek-cut certificate.subject certificate.issuer certificate.not_valid_after | head -20
```

Perguntas:
1. Alguma conexão usa TLS 1.0, 1.1 ou SSL? Para qual serviço?
2. Existem conexões TLS sem SNI? Para onde vão?
3. Quais emissores de certificado aparecem (Let's Encrypt, DigiCert etc.)?

### Texto claro (sem criptografia)

```bash
cd ~/projeto-captura/zeek-tsv

# HTTP puro
cat http.log | zeek-cut host uri method status_code user_agent | head -30

# serviços clássicos em texto claro: portas comuns
cat conn.log | zeek-cut id.resp_h id.resp_p proto \
  | awk '$2==21 || $2==23 || $2==25 || $2==110 || $2==143 || $2==161' | sort | uniq -c
```

Depois, no **Wireshark**:

```
http || ftp || telnet || smtp || pop || imap || snmp || ldap
http.authorization
http.request.method == "POST"
```

Use *Follow → TCP Stream* nas conexões suspeitas e *File → Export Objects → HTTP* para ver arquivos transferidos.

Perguntas:
1. Existe HTTP puro? Pode ser portal cativo, teste de conectividade, atualização antiga ou dispositivo IoT?
2. Há credenciais ou dados sensíveis visíveis? (Se houver e forem **seus**, anote o aprendizado. Se forem de terceiros, não use nem divulgue.)

### Dispositivos e serviços da rede local

```bash
cd ~/projeto-captura/zeek-tsv

# DHCP: quem recebeu IP e com qual nome
cat dhcp.log | zeek-cut mac host_name assigned_addr lease_time

# tráfego de descoberta local por porta
cat conn.log | zeek-cut proto id.resp_p | awk '$2==1900 || $2==137 || $2==5353 || $2==5355' | sort | uniq -c
```

No Wireshark: `arp`, `ssdp`, `nbns`, `mdns`, `llmnr`, `icmpv6`. Ative a resolução de endereços físicos (*View → Name Resolution → Resolve Physical Addresses*) para ver os fabricantes pelo MAC.

Perguntas:
1. Quais dispositivos e fabricantes aparecem no ARP, DHCP e mDNS?
2. Algum serviço se anuncia na rede (impressora, TV, compartilhamento)?
3. Quais serviços de descoberta (SSDP, NBNS) estão ativos?

### Anomalias

```bash
cat weird.log | zeek-cut name | sort | uniq -c | sort -rn
cat notice.log 2>/dev/null | head
```

No Wireshark: *Analyze → Expert Information* e o filtro `tcp.analysis.flags`.

Perguntas:
1. Quais comportamentos o Zeek marcou como estranhos? São problemas reais ou efeitos da captura (offload, pacotes perdidos pela placa)?
2. Há retransmissões, resets ou janelas zero? Em quais conexões?

### Análise com pandas

```python
import pandas as pd

conn = pd.read_json("conn.log", lines=True)
conn["ts"] = pd.to_datetime(conn["ts"], unit="s")

# tráfego por minuto
(conn.set_index("ts")[["orig_bytes", "resp_bytes"]]
     .resample("1min").sum().plot(figsize=(10, 4)))

# top destinos
conn.groupby("id.resp_h")[["orig_bytes", "resp_bytes"]].sum().sort_values("resp_bytes", ascending=False).head(10)

# duração das conexões
conn["duration"].describe()
```

Ideias de gráficos para o projeto: tráfego ao longo do tempo, top 10 domínios, distribuição de versões TLS, proporção de protocolos, histograma de duração de conexões.

## Dicas e cuidados

- Sempre rode o Zeek **em uma pasta nova** para não misturar logs.
- Guarde o **pcap original intocado**; trabalhe com cópias.
- Use `-C` ao ler capturas feitas na sua própria placa.
- Instale pacotes de scripts da comunidade do Zeek para testar.
- Capturas podem conter dados pessoais. Não compartilhe sem anonimizar e siga a política da instituição e a LGPD;

