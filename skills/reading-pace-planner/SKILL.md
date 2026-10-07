---
name: reading-pace-planner
description: >
  Divide um livro ou PDF longo em blocos diários de leitura até uma data-alvo, respeitando o ritmo de leitura e o
  tempo disponível informados pelo usuário. Ative quando o usuário disser "quero terminar esse livro até tal data",
  "monta um plano de leitura", ou informar um material longo com prazo.
---

# Skill: Reading Pace Planner

## Visão geral

Livros e PDFs longos costumam ser abandonados não por falta de interesse, mas por falta de um plano realista de
quando ler cada parte. A skill divide o material em blocos diários/semanais até a data-alvo, respeitando quanto
tempo o usuário realmente tem disponível.

## Passo 1 — Levantar os parâmetros

Confirme: tamanho do material (páginas/capítulos), data-alvo, e quanto tempo por dia/semana o usuário tem
disponível para ler.

## Passo 2 — Calcular o ritmo necessário

Calcule quantas páginas/capítulos por sessão são necessários para terminar na data, e avise claramente se o ritmo
exigido é irreal para o tempo disponível informado, em vez de gerar um plano impossível de seguir.

## Passo 3 — Respeitar a estrutura do material

Divida por capítulo ou seção sempre que possível, evitando cortar no meio de uma unidade lógica só para bater a
página exata calculada.

## Passo 4 — Entregar o plano

Apresente como um calendário com o que ler em cada sessão, e ofereça lembrar cada bloco por scheduled task se o
usuário quiser esse acompanhamento.

## Coisas que esta skill nunca faz

- Nunca esconde que o ritmo calculado é inviável — avisa e sugere um novo prazo ou mais tempo por sessão.
- Nunca assume o mesmo ritmo de leitura para materiais de dificuldade muito diferente sem perguntar.
