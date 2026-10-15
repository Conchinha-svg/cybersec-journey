# Dia 1 / Day 1 (14/10/2026)

[Português](#português) | [English](#english)

---

# Português

## Dia 1: Fundamentos e análise de tráfego

### Parte 1: Teoria (TryHackMe, Pre Security)

Objetivo: nivelar conhecimentos básicos de cibersegurança.

| Sala | O que fiz | Lição |
| --- | --- | --- |
| Offensive Security (Red Team) | Simulei um ataque: escaneei um site, encontrei diretórios ocultos e contornei um login | Perceber o atacante para defender melhor |
| Defensive Security (Blue Team) | Analisei logs num painel SIEM, identifiquei um ataque de força bruta e bloqueei o IP | A defesa exige visibilidade e capacidade de reação |
| Careers in Cyber | Visão geral das carreiras (Pentester, SOC, Forense, GRC) | A área é vasta e não se resume a "hackear" |
| Inside a Computer System | Hardware (CPU, RAM, BIOS) e processo de boot | É preciso saber como a máquina funciona para a proteger |
| Mystery Chest e Recap | Desafios de lógica e decodificação (Base64 e binário) e revisão final | Consolidar o que vi no dia |

### Parte 2: Prática (Wireshark)

Objetivo: capturar e analisar tráfego de rede real.

**Configuração:** interface Wi-Fi, captura de cerca de 30 segundos, com o browser aberto em `example.com`.

#### Análise DNS

- O site `example.com` resolveu para dois IPs: `104.20.23.154` e `172.66.147.243`.
- Total de **72 pacotes DNS** na captura, incluindo tráfego de fundo (Spotify, Mozilla).
- O DNS traduziu o nome do site para endereços IP. Receber dois IPs sugere que o serviço está distribuído por vários servidores.

#### Filtros e protocolos

- Filtro `http`: **0 pacotes**. Era esperado, porque o tráfego moderno usa HTTPS (cifrado).
- Filtro `tcp`: consegui ver o handshake de três passos numa ligação HTTPS.

#### Handshake TCP (exemplo)

| Passo | Origem | Destino |
| --- | --- | --- |
| SYN | 192.168.x.x:51019 | 3.173.21.63:443 |
| SYN-ACK | 3.173.21.63:443 | 192.168.x.x:51019 |
| ACK | 192.168.x.x:51019 | 3.173.21.63:443 |

Ligação estabelecida com sucesso na porta 443 (HTTPS).

### O que aprendi

- O HTTPS esconde o conteúdo, mas **não esconde tudo**: os pedidos DNS e os IPs de destino continuam visíveis. Para um analista de SOC, isso já diz muito sobre quem fala com quem.
- Uma captura de 30 segundos já mostra muito tráfego de fundo de aplicações que nem sabia que estavam a comunicar.

### Segurança

O ficheiro `dia1-captura.pcapng` ficou guardado só localmente e **não foi publicado**, porque pode conter dados da minha rede.

### Dúvidas para pesquisar

- Porque é que o `example.com` devolve dois IPs? (balanceamento de carga, CDN?)
- Que informação continua visível numa ligação HTTPS além do DNS e do IP?

---

# English

## Day 1: Fundamentals and traffic analysis

### Part 1: Theory (TryHackMe, Pre Security)

Goal: build a baseline of cybersecurity fundamentals.

| Room | What I did | Takeaway |
| --- | --- | --- |
| Offensive Security (Red Team) | Simulated an attack: scanned a website, found hidden directories and bypassed a login | Understand the attacker to defend better |
| Defensive Security (Blue Team) | Analysed logs in a SIEM dashboard, identified a brute-force attack and blocked the IP | Defence needs visibility and the ability to react |
| Careers in Cyber | Overview of career paths (Pentester, SOC, Forensics, GRC) | The field is broad and is not just "hacking" |
| Inside a Computer System | Hardware (CPU, RAM, BIOS) and the boot process | You need to know how a machine works to protect it |
| Mystery Chest and Recap | Logic and decoding challenges (Base64 and binary) and a final review | Consolidate what I learned today |

### Part 2: Hands-on (Wireshark)

Goal: capture and analyse real network traffic.

**Setup:** Wi-Fi interface, roughly 30 seconds of capture, with the browser open on `example.com`.

#### DNS analysis

- `example.com` resolved to two IPs: `104.20.23.154` and `172.66.147.243`.
- **72 DNS packets** in total, including background traffic (Spotify, Mozilla).
- DNS translated the site name into IP addresses. Getting two IPs suggests the service is spread across several servers.

#### Filters and protocols

- `http` filter: **0 packets**. Expected, since modern traffic uses HTTPS (encrypted).
- `tcp` filter: I was able to see the three-way handshake on an HTTPS connection.

#### TCP handshake (example)

| Step | Source | Destination |
| --- | --- | --- |
| SYN | 192.168.x.x:51019 | 3.173.21.63:443 |
| SYN-ACK | 3.173.21.63:443 | 192.168.x.x:51019 |
| ACK | 192.168.x.x:51019 | 3.173.21.63:443 |

Connection successfully established on port 443 (HTTPS).

### What I learned

- HTTPS hides the content, but **not everything**: DNS requests and destination IPs are still visible. For a SOC analyst, that already says a lot about who is talking to whom.
- A 30-second capture already reveals plenty of background traffic from apps I did not know were communicating.

### Security

The file `dia1-captura.pcapng` is stored locally only and was **not published**, because it may contain data from my network.

### Questions to research

- Why does `example.com` return two IPs? (load balancing, CDN?)
- What information is still visible in an HTTPS connection besides DNS and the IP?
