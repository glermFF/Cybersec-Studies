Neste curso disponibilizado pela Cisco, tive a experiência de aprender habilidades e conceitos de nível intermediário voltados a defesa de ativos de uma rede computacional, dos quais entram neste conjunto equipamentos, softwares e dados. 

Seu cronograma ficou dividido da seguinte maneira:

	1 - Compreendendo Defesa: Conceituando sobre políticas de privacidade, Níves de Defesa, Categorizando ativos dentro de uma empresa, Vulnerabilidades e maneiras de mitigar suas falhas, Tipos de Ameaças, Usando Packet Tracer;
	2 - Defesa do Sistema e Rede: Segurança Física, Segurança em Aplicações, Blindagem de rede e de seus dispositivos, Importância Cybersegurança, Sistemas embarcados;
	3 - Aprendizagem sobre formas de controle de acesso na rede, filtrando o acesso para cada tipo de funcionário, identificar atividade dos usuários na rede, entendendo sobre a operação AAA(autenticação, autorização, contabilidade);
---
## 2 . Defesa de Sistemas e da Rede

### Segurança Física

	A segurança física é a primeira camada de defesa implementada dentro de uma organização e em alguns casos é a camada que tem maior investimento, usando principalmente as pessoas no seu reforço.
	Os principais métodos implementados envolvem construção de barreiras delimitando uma área importante, cercas elétricas, seguranças, câmeras de vigilância, cartões de acesso, uso de biometria (digital, face, íris, voz), etc.
	Estes meios e camadas podem ser utilizadas por sistemas e equipamentos, como no caso da biometria, utilizada em aplicativos de banco e para o desbloqueio do aparelho.

### Segurança de Aplicações

	Vamos do início, na parte de desenvolvimento da aplicação onde nesta etapa pode ser que alguma vulnerabilidade seja implementada, por acidente ou não. Evitar e assegurar aplicações destas falhas, principalemente os sistemas que trabalham com dados é crucial e veremos formas de como podemos melhorar esta segurança.
	A principal delas e a criptografia. Funções baseadas em Hash são as principais para tal tarefa, encriptando os dados no momento que o sistema os recebe por suas entradas. Juntamente disso, podemos implementar formas de verificação destes dados encriptados, para casos de possíveis violações. Das funções hash mais usadas: MD5, SHA-1, SHA-256.

### Segmentação

	A ideia de segmentação de rede consiste em subdividi-la em camadas direcionando o tráfego de dados entre cada dispositivo de cada setor melhorando o desempenho e aumentando a seguranaça ponta a ponta.
	Os dispositivos de cada setor ficam conectados a uma VLAN's, fazendo o dispositivo pensar que está conectado em uma mesma rede que os demais por meio de conexões lógicas entre cada um deles. Os adiminstradores de uma VLAN separa os dispositivos por exemplo, entre função, setor e atividade.
	Temos a abordagem das DMZ's ou Zonas Desmilitarizadas, 

### Segurança em Redes WLAN

	Em redes wireless, há configurações recomendadas com realção a sua codificação por meio de protocolos de segurança já inclusos nas configurações dos roteadores. 
	A principal maneira é utilizar a configuração WPA/WPA-PSK/WPA2/WPA3, que utilizam encriptação em suas senhas dificultando ataques de força bruta utilizada por crackers. 

### Importância da Cybersegurança

	Inspeções de segurança devem ser levadas em consideração nas empresas para a checagem de possíveis falhas e pontos que agregam uma vulnerabilidade a mais que pode acarretar em um futuro ataques. Não somente, certificar de que todo o sistema ou grande parte dele não será comprometido em um cenário onde um ataque ocorre. Verificar atualizações de software, se não há um endpoint aberto, configruações de firewall, sistemas para casos de falhas de energia, devem ser levados em consideração em uma inspeção.
	Seu sistema computacional deve estar sempre preaprado a erros, então implementar um tipo de rede descentralizada onde não há um meio principal que os equipamentos dependam para permanecerem conectados. Há protocolos, como o xxx, que ao ocorrer uma interrupção de um switch ou um cabo seja partido, a comunicação desta rede é redirecionada para outro switch dentro dela e habilitando uma porta que sirva de substituto para o caminho interrompido.
	Com relação a dados, backups são essenciais a serem feitos. Perca de informações após um ataque hacker, falha no dispositivo, acidentes cometidos por um funcionaŕio podem ocorrer e é necessário que estes dados sejam recuperados de alguma maneira. Backups podem ser feitos por um sistema em nuvem, por um servidor e da maneira, digamos "classica", por um HD de backup criptografado.

### Sistemas embarcados

	A integração de dispositivos IoT estão cada vez mais presentes em redes de organizações com funções específicas para seu uso. Podem ser usados no monitoramento da própria rede computacional, controle de estoque, uso de sensores que enviam informações de acesso físico dentro de um estabelecimento e na comunicação entre outros IoT's dentro de um sistema, gerando um tráfego de dados com maior otmização.
	Integrar mais dispositivos dentro de um sistema/rede leva à abertura de vulnerabilidades a rede. Ataques de botnet podem ocorrer com maior facilidade em uma rede de embarcados caso a prevenção necessária e a verificação de segurança destes dispositivos não forem configuradas corretamente. Um caso que ocorreu no ano de 2016 com bábas eletrônicas é um bom exemplo disso.
	Não é preciso ter habilidades específicas para efetuar um ataques do tipo. Softwares como o Shodan, um motor de busca que consegue encontrar sistemas/dispositivos embarcados com um tipo de falha apenas pela internet, pode ser usado por qualquer pessoa com o mínimo de curiosidade no assunto.

---
## 3. Controle de Acesso

### Controle de Acesso

	O controle de acesso pode ser de tipos variados dependendo de onde será feita a implementação. Para evitar acesso não autorizado em um banco, é feito o controle de acesso físico por meio de várias camadas como seguranças, detectores de metais, de movimento e câmeras de vigilância.
	Temos os métodos de controle de acesso lógicos que envolvem uso de criptografia de dados, SOCs, firewalls na rede, uso de protocolos para determinado setor da rede e lista de controle de acesso (os switches e roteadores possuem algo parecido mas pode ser combinado com um firewall ou outro software).
	Por Último, o controle de acesso administrativo envolvem certas etapas e "camadas" como meio de filtro para funcionários e usuários que entram e saem de uma organização, por exemplo, apresentação da política de intenções que devem ser seguida, processos seletivos minuciosos e uma análise crítica de perfil da pessoa. O método de controle de autenticação usado por org's que unem o que uma pessoa é, o que ela tem e o seu conhcecimento. Como se gerasse uma camada a mais de segurança por meio de cada funcionário registrado.

#### Os 3A's

	xxxx

