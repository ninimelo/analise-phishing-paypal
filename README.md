# 🕵️‍♂️ Relatório de Investigação SOC: Phishing PayPal

Este projeto simula uma triagem real realizada por um Analista SOC (Blue Team) ao investigar um e-mail suspeito relatando a suspensão de uma conta do PayPal.

---

## 📧 1. O E-mail Analisado (Amostra de Teste)
Abaixo está o cabeçalho e corpo do e-mail suspeito que foi interceptado:

```text
Return-Path: <service-update@paypaI-security-alert.com>
Received: from mail-server.attacker-node.xyz (unknown)
        by ://google.com with ESMTPS id p11si543219qkb.64
        for <vitima@empresa.com.br>;
Subject: Urgente: Sua conta do PayPal foi temporariamente suspensa!
Date: Fri, 16 Oct 2026 10:15:22 -0300
From: "Suporte PayPal" <service-update@paypaI-security-alert.com>
To: vitima@empresa.com.br

Prezado cliente,
Clique no link abaixo para confirmar seus dados bancários:
http://shorturl.at (Atualizar Dados Agora)
```

---

## 🔍 2. Análise Técnica e Evidências

Durante a triagem, foram identificados os seguintes fatores de risco (IoCs):

*   **Fraude Visual (Typosquatting):** O remetente exibe o nome "Suporte PayPal", mas o domínio real é `paypaI-security-alert.com`. O atacante trocou a letra **"l"** minúscula por um **"I"** maiúsculo para enganar a vítima a olho nu.
*   **Inconsistência de Origem:** O servidor que realizou o disparo foi o `mail-server.attacker-node.xyz`, provando que o e-mail não possui relação com os servidores legítimos do PayPal.
*   **Ocultação de Link:** O corpo do e-mail utiliza um encurtador de URL (`shorturl.at/xyz789`) para mascarar o verdadeiro site de destino, tática clássica para burlar filtros de segurança e realizar roubo de credenciais (Credential Harvesting).

---

## 🛡️ 3. Conclusão e Mitigação Recomendada

**VEREDITO:** 🚨 **MALICIOSO** (Tentativa de Phishing).

Como resposta ao incidente, as seguintes ações defensivas foram recomendadas para o time de infraestrutura:
1. **Bloqueio de Domínio:** Inserir o domínio fraudulento `paypaI-security-alert.com` na lista de rejeição (Blocklist) do Gateway de e-mail corporativo.
2. **Filtro de Navegação:** Bloquear o acesso à URL encurtada `shorturl.at/xyz789` no Proxy/Firewall da empresa para evitar que funcionários cliquem por engano.
