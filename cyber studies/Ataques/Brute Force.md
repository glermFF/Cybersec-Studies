# Ataque de Força Bruta

Um ataque de força bruta tenta obter acesso a uma conta ou recurso testando sistematicamente combinações de senhas (ou chaves) até encontrar a correta. Ataques modernos combinam técnicas (dicionários, listas de senhas vazadas, ataques distribuídos) e costumam ser automatizados por bots ou ferramentas especializadas.

---
## Tipos de Ataques

- Brute force simples — tentativa exaustiva de todas as combinações possíveis (cara a cara). É pesado e lento, mas garantido se o espaço de senhas for pequeno. 
- Dictionary attack (ataque por dicionário) — usa listas de senhas comuns, variações e palavras de dicionários para acelerar o processo.
- Hybrid attack — combinação de dicionário + mutações (ex.: adicionar números, símbolos).
- Reverse brute force — testa senhas «populares» contra muitos usuários (útil quando a senha fraca é comum).
- Rainbow table attack — acelera quebra de hashes usando tabelas pré-computadas (time–memory trade-off). Ferramentas como *Ophcrack* e *RainbowCrack* implementam variações dessa técnica.
- Credential stuffing — reutilização de pares usuário/senha obtidos em vazamentos anteriores (não é brute force puro, mas tem objetivo semelhante). 
- Password spraying — o atacante tenta *uma senha fraca* em muitos usuários para evitar bloqueios por tentativas falhas em uma única conta. Muito usado contra ambientes corporativos.

---

## +Detalhes

### Password Spraying

Definição: técnica onde o atacante testa *uma senha comum* (ex.: `Summer2023!`) contra muitos usuários para não gerar bloqueios por tentativas repetidas em uma mesma conta. Funciona porque muitas contas usam senhas previsíveis. 
Detecção: picos de tentativas de login com o mesmo password vindo de diversos IPs / localidades; tentativas contra contas inativas ou de baixo uso.  
Contra-medidas recomendadas: implementação de MFA, monitoramento de padrões de falha (e.g., mesmo password sendo usado em várias contas), bloqueio inteligente por IP/país, política de senhas e verificação de credenciais comprometidas. 

---

## Medidas de mitigação gerais contra brute force (resumo prático)

1. Autenticação multifator (MFA/2FA) para todos os acessos privilegiados e, idealmente, para usuários comuns. MFA reduz drasticamente o sucesso de ataques de força bruta/credential stuffing. 
2. Rate limiting / bloqueio inteligente: implemente limitação de tentativas e lockouts adaptativos (ex.: lockout temporário, janela observacional, blacklists) conforme orientação do NIST SP 800-63B. Evite permitir infinitas tentativas. 
3. Detecção e resposta: monitorar logs de autenticação, criar alertas para padrões de password spraying, credential stuffing e tentativas de login em massa. 
4. Política de senhas fortes + bloqueio de senhas vazadas: negar senhas comumente usadas e checar credenciais contra bases de vazamentos (breach corpuses). 
5. Proteção de endpoints e mobile: defender contra trojans que realizam MitB/MitM e prevenir instalações de APKs/softwares não verificados. 
6. Armazenamento seguro de senhas: usar hash fortes (Argon2/Bcrypt/Scrypt) com salt; não armazenar senhas em texto ou com algoritmos débeis. (boa prática geral de segurança). 
7. Captcha/step-up em fluxos críticos quando detecção de risco for identificada (p.ex. tentativas repetidas, geolocalização suspeita). 

---

## Referências

> - [Brute Force Attack](https://owasp.org/www-community/attacks/Brute_force_attack?utm_source=chatgpt.com)
