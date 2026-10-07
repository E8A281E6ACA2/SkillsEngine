---
name: concept-map-builder
description: >
  Gera um mapa conceitual (em texto estruturado ou diagrama mermaid) mostrando como os conceitos de um tema ou
  capítulo se conectam entre si. Ative quando o usuário disser "monta um mapa conceitual disso", "quero ver como
  esses conceitos se relacionam", ou pedir uma visão geral visual de um tema.
---

# Skill: Concept Map Builder

## Visão geral

Um mapa conceitual mostra relações que um resumo em texto corrido esconde: o que depende do quê, o que é oposto ao
quê, o que é caso particular de um conceito mais geral. A skill lê o material e monta esse mapa, priorizando as
relações mais importantes para entender o tema como um todo, não listando conceitos soltos.

## Passo 1 — Extrair os conceitos centrais

Identifique de 6 a 15 conceitos centrais do material — nem tão poucos que percam nuance, nem tantos que o mapa
fique ilegível.

## Passo 2 — Definir as relações

Para cada par de conceitos relacionados, defina o tipo de relação (depende de, é oposto a, é exemplo de, causa,
é parte de) — relações vagas tipo "se relaciona com" devem ser evitadas ou especificadas melhor.

## Passo 3 — Montar o mapa

Gere o mapa como diagrama mermaid (quando o formato de saída suportar) ou como uma lista hierárquica de relações
quando não suportar, sempre legível sem precisar de ferramenta externa.

## Passo 4 — Explicar as relações não óbvias

Depois do mapa, destaque em 2-3 frases as relações menos óbvias que ele revela, para o usuário não passar batido
pelo insight mais útil do mapa.

## Coisas que esta skill nunca faz

- Nunca cria relação entre conceitos que não existe de fato só para o mapa parecer mais rico.
- Nunca substitui o material original — é um resumo visual, não a fonte completa.
