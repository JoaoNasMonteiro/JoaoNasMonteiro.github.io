---
layout: project_page       # Importante: Usa o layout com a timeline automática
title: "Nome do Projeto"   # Ex: Bot de Automação IoT
description: "Uma frase curta de efeito para o card na página de projetos."
status: "Em Andamento"     # Opções: Em Andamento | Concluído | Arquivado (Afeta a cor do badge)
tech_stack: [Python, C++, Raspberry Pi, MQTT] # Lista de tecnologias (aparecem como tags)
repo_url: "https://github.com/SeuUsuario/repo" # Link para o código fonte
priority: 1                # (Opcional) Para ordenar: 1 aparece primeiro
slug: meu-projeto-x        # A CHAVE DE LIGAÇÃO (Deve ser igual ao campo 'project' nos posts)
---

## 💡 Sobre o Projeto
Explique o "porquê" deste projeto existir. Qual problema ele resolve? É apenas para estudo ou tem uso real?

> **Objetivo:** Criar um sistema autônomo que não deixe minhas plantas morrerem de sede.

## 🛠️ Arquitetura e Hardware
Se for hardware/IoT, liste os componentes. Se for software, explique a estrutura.

* **Microcontrolador:** ESP32
* **Sensores:** Capacitivo de umidade de solo
* **Backend:** Server em Python rodando num Raspberry Pi Zero

![Diagrama do Sistema](/assets/images/nome-da-imagem.png)
*(Legenda: Diagrama de blocos do sistema)*

## 🚀 Desafios Técnicos
O que está sendo difícil? Onde está a complexidade?
* Gerenciar o consumo de energia da bateria.
* Latência na comunicação MQTT.

## 📸 Galeria
Coloque screenshots ou fotos do protótipo aqui.

## 🔗 Links Úteis
* [Documentação da API](link)
* [Datasheet do Sensor](link)