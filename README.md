# Captura e Análise de Tráfego DNS com tcpdump

Este repositório documenta um mini home lab focado na **captura e análise de tráfego DNS** utilizando a ferramenta **tcpdump** em ambiente Linux.  
O objetivo é demonstrar, de forma simples e prática, a capacidade de **capturar pacotes de rede, interpretar consultas DNS e compreender o fluxo de resolução de nomes**, habilidades essenciais para analistas SOC e profissionais de segurança da informação.

---

## 🎯 Objetivo do Lab

- Capturar tráfego DNS na porta 53 (UDP)
- Gerar consultas DNS de forma controlada
- Analisar pacotes DNS (queries e responses)
- Interpretar registros A e AAAA
- Demonstrar entendimento prático de tráfego de rede

---

## 🧪 Ambiente Utilizado

- Sistema operacional: Linux (Ubuntu)
- Rede: ambiente virtual com IP privado
- Interface de rede: `ens33`

  ![print](https://github.com/user-attachments/assets/da5bc6c2-0c49-4732-bd5e-14dfa2363641)

- Ferramentas:
  - tcpdump
  - curl
  - tshark (para leitura alternativa)

---

## 🔍 Metodologia

### 1️⃣ Início da captura DNS

A captura foi realizada filtrando tráfego UDP na porta 53:

```bash
sudo tcpdump -i ens33 udp port 53 -w capture_dns.pcap
```


### 2️⃣ Geração de tráfego DNS

Enquanto a captura estava ativa, foi feita uma requisição HTTPs para gerar resolução DNS:

```bash
curl https://google.com
```
### 3️⃣ Finalização da captura

Após a geração do tráfego, a captura foi encerrada:
```bash
sudo pkill tcpdump
```

<img width="845" height="279" alt="image" src="https://github.com/user-attachments/assets/37a8346e-3feb-4c3b-9373-efee556c4aec" />

### 4️⃣ Leitura e análise dos pacotes capturados

Os pacotes foram analisados diretamente no terminal:
```bash
sudo tcpdump -r capture_dns.pcap -vv
```

<img width="1127" height="200" alt="image" src="https://github.com/user-attachments/assets/d1c07975-527c-42b0-b4b4-a9470b5c47ae" />

### Também foi utilizada a leitura via tshark:
```bash
tshark -r capture_dns.pcap
```

<img width="1044" height="103" alt="image" src="https://github.com/user-attachments/assets/75046069-d639-45bc-abf7-5d930b14e532" />

### 📦 Resultados Observados

Durante a análise, foram identificados:

Consultas DNS do tipo A (IPv4) para google.com

Consultas DNS do tipo AAAA (IPv6) para google.com

Respostas DNS válidas contendo endereços IPv4 e IPv6

Fluxo completo de resolução DNS (query → response)

Esse comportamento indica resolução de nomes legítima e esperada, sem indícios de atividade maliciosa.
