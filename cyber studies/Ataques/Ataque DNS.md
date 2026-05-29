# Ataque DNS

## Sobre

Servidores DNS são grandes pilares no funcionamento da rede internet possibilitando a criação de alias dos servidores e hosts em nomes de domínio possibilitando uma melhor forma de gravar e buscar por um endereço de um site, por exemplo, ao invés de digitar todo seu endereço IP. Os domínios são organizados por entidades domínios como o NIST.

Configurado de maneira crua, o resolvedor DNS se torna vulnerável o suficiente para simples ataques ao seus serviços levando a roubo de dados e interrupção de operações escalando para ataques DoS (Denial of Service).

## Tipos de ataques

### DNS FAKE ou DNS Spoofing

Funciona por meio de injeção de endereços dentro do cache do resolvedor DNS mudando o comportamento para a página que um host pretende acessar, enviando a sua requisição para armadilhas de roubo de dados. No ataque, o endereço adcionado ao cache são de sites confiáveis clonados/simulados enganando o usuário dando brecha para roubo dos dados e monitoramento de suas atitudes por pensar que está acessando o site que sempre utilizou.

Essa forma de ataque é conhecida como Man-in-the-Middle (MITM) com o atacante operando como intermédiario do tráfego roubando informações dos pacotes.

### Sequestro (DNS Hijacking)

A forma de operação neste cenário pode lembrar o DNS Fake com a diferença do objetivo principal, o servidor DNS. No anterior, o foco no envio de requisições falsas a fim do resolvedor DNS repassar para outros hosts, sendo de fácil detecção e pode ser solucionado em instantes. Por aqui muda, o invasor mira direto no servidor DNS possibilanto a reconfiguração dos domínios registrados e redirecionando o tráfego para outros *links*.

Em cenários de sucesso do ataque o servidor DNS alterado pode permanecer por bom tempo sem que sejam detectadas as alterações, explodindo uma grande bomba de problemas para a organização afetada.

A base para o ataque utilizade técnicas de [Engenharia Social](../Ataques/Engenharia%20Social.md)

### Tunelamento DNS

Dentre as variantes de ataque DNS ess pode ser o mais sofisticado. O tunelamento consiste na criação de payloads úteis em um canal clandestino de comunicação explorando a infraestrutura do DNS. Seu funcionamento é eficiente sendo imperceptível por sistemas de tráfego de dados e/ou por IDS's.

## Detecção 

Entendemos as possíveis maneiras de prejudicar o DNS e convergir seus dados a servidores não confiavéis que não tem nenhuma ligação com os domínio original, mas como identificar tal incosistência? Softwares voltados ao monitoramento de tráfego da rede são nossos maiores aliados para isso. Abaixo uma pequena ideia de como utiliza-los para este fim.

**Snifeers ([WireShark](../Ferramentas/WireShark.md))**

Os sniffers permitem uma análise profunda do payload dos pacotes, o que é fundamental para identificar o tunelamento de DNS, podendo verificar dados codificados anexados a nomes de subdomínios ou inseridos em registros. Isso ao fato que certas ferramentas deixam assinaturas claras facilita ainda mais a identificação de ataques.

No caso do WireShark, seu trabalho em dividir os dados permite aos analistas observarem fragmentos de pacotes exfiltrados nas consultas DNS, evidênciando as anomalias na comunicação.

**IDS**

IDSs podem detectar exfiltração, mas enfrentam certas limitações. Os tradicionais focam em padrões conhecidos e assinaturas, então caso o atacante utilizar um domínio novo ou uma técnica que não gere um alerta específico, ele pode passar por debaixo dos panos e não ser bloqueado.

### Proteção

Servidores DNS por padrão com foco apenas em ser funcional não apresentam boas configurações de segurança, levando a exposição da infraestrutura a diversos problemas de segurança a. Como forma de resolver tais questões protegendo o tráfego de dados temos o DNSSEC. 

O DNSSEC (Domain Name System Security Extensions) é um conjunto de extensões de segurança projetadas para proteger o protocolo DNS contra ataques de falsificação funcionando como uma camada adicional de autenticação criptografando a chave pública para assinar digitalmente os registros da zona DNS. Sua aplicação permite que resolvedores DNS possam autenticar a origem das requisições, verificar assinatura digital antes de armazenar informações e mitigar ataques MITM.

Não é uma bala de prata. Então ele não deixa o resolvedor DNS imune de qualquer tipo de ataque podendo ainda ser sucetível a ataques de tunelamento, pois seu foco é na integridade e autenticidade.

## Referências

- [Um guia abrangente para ataques de DNS](https://www.startupdefense.io/pt-br/blog/um-guia-abrangente-para-ataques-de-dns-protegendo-a-infraestrutura-da-internet)
- [Como Funciona o DNSSEC](https://www.cloudflare.com/pt-br/learning/dns/dnssec/how-dnssec-works/)
- [Guia de Utilização do DNSSEC](https://www.ish.com.br/wp-content/uploads/2023/03/Utilizacao-do-DNSSEC-para-proteger-contra-ataques-DNS-Spoofing.pdf)