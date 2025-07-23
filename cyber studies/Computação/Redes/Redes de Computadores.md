Uma rede de computadores é uma estrutura de comunicação que por meio de protocolos e equipamentos de rede é feita a conexão e comunicação entre outros dispositivos que conseguem se conectar por meio de uma placa de rede.

A internet é uma rede de computadores, a maior que existe no mundo. Por meio dela varias redes se conectam entre si por meio do protocolo ethernet.

## Introdução

A comunicação entre dispositivos em uma rede é feita por meio de envio de pacotes de dados. Nestes pacotes estão informações do dispositivo que enviou a mensagem, o destino dela, o tamanho, tipo de requisição, etc. Para a mensagem é preciso um intermediador, um tipo de "carteiro" que encaminhará a mensagem ao seu devido destino, neste caso, será o roteador.
Por meio dele, vários dispositivos podem conectar e comunicar entre os outros sem colisão ou desvio de pacotes/dados no tráfego.

Há outros dispositivos intermediadores: HUB e Switch. O Hub não é mais recomendado atualmente por problemas com relação a segurança e com o envio de pacotes.


### "Alterando" o MAC

Por meio do ifconfig é possível alterar os valores do endereço MAC encontrados nele:

``` bash
1$ ifconfig wlan0 down
2$ ifconfig wlan0 hw ether 00:00:00:00:00:00
3$ ifconfig wlan0 up
```

Esse valor é revertido para o valor original do MAC ao reiniciar o computador/VM.

! Endereço MAC é o endereço físico em uma placa de rede. Sendo assim, na prática, estamos alterando oque o SO está registrando como respectivo MAC do aparelho que editamos !

**Modo Monitor**:
``` bash
1$ ifconfig wlan0 down
2$ iwconfig wlan0 mode monitor
3$ ifconfig wlan0 up
```

**Verificando redes sem fio disponíveis:**
```bash
user@bash iw dev {sua_interface} interface add mon0 type monitor;
```


---
[[Redes Wireless]] [[Protocolos de Rede]]