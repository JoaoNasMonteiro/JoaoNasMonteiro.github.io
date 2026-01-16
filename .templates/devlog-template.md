---
layout: post
title: "DevLog Template"
date: 2026-01-15 10:00:00 -0300
categories: [DevLog, to-be-done]    # Categorias gerais
tags: [to-be-done]        # Tags específicas
project: test-project            # CRÍTICO: Deve ser idêntico ao 'slug' do projeto
---

**Mood:** 😤 Frustrado | **Música:** Daft Punk - Tron Legacy

## CONTEXTO
O que eu estava tentando fazer hoje? 
Ex: "Hoje tentei calibrar o sensor de umidade, mas os valores estavam flutuando muito."

## 🐛 O PROBLEMA
Descreva o erro técnico. Copie o erro do terminal se houver.

```python
# O código que estava dando erro
def ler_sensor():
    valor = analog_read(34)
    return valor # Estava retornando 0 sempre