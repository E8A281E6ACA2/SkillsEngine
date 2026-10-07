---
name: jargon-buster
description: >
  Dado um texto de uma área nova para o usuário, extrai só os termos técnicos desconhecidos e explica cada um de
  forma curta, sem reescrever o texto inteiro. Ative quando o usuário disser "não conheço esses termos", "explica
  o jargão desse texto", ou colar um texto técnico de área desconhecida pedindo esclarecimento pontual.
---

# Skill: Jargon Buster

## Visão geral

Diferente de simplificar o texto inteiro, o Jargon Buster resolve um problema mais pontual: o texto em si é
compreensível, só os termos técnicos específicos da área é que travam a leitura. A skill identifica só esses
termos e explica cada um brevemente, mantendo o texto original intacto.

## Passo 1 — Ler o texto e identificar jargão

Percorra o texto e liste os termos técnicos ou específicos da área que provavelmente não são de conhecimento geral
— nomes próprios de conceito, siglas, termos emprestados de outro idioma sem tradução comum.

## Passo 2 — Explicar cada termo

Para cada termo, dê uma explicação curta (1-2 frases) no contexto em que ele foi usado no texto — não uma
definição de dicionário genérica, mas o que ele significa *ali*.

## Passo 3 — Organizar como glossário lateral

Entregue como uma lista de termo → explicação, na ordem em que aparecem no texto, para o usuário consultar enquanto
lê o original.

## Coisas que esta skill nunca faz

- Nunca reescreve ou resume o texto original — só anota os termos.
- Nunca lista termos que já são de conhecimento geral só para parecer mais completo.
