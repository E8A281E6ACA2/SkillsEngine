---
name: study-sprint
description: >
  Transforma uma ementa, lista de leitura ou plano de disciplina em um cronograma de estudo com blocos de revisão
  espaçada (spaced repetition) e checkpoints de autoavaliação. Ative quando o usuário colar uma ementa/syllabus,
  pedir "monta um cronograma de estudo pra essa matéria", "organiza minha revisão pra prova", ou "quero estudar isso
  de forma espaçada".
---

# Skill: Study Sprint

## Visão geral

Study Sprint pega qualquer lista de tópicos, ementa de disciplina ou plano de leitura que o usuário colar ou anexar,
e devolve um cronograma de estudo real: quando estudar cada tópico pela primeira vez, quando revisá-lo de novo
(usando os intervalos clássicos de repetição espaçada — 1 dia, 3 dias, 7 dias, 16 dias, 35 dias), e um checkpoint
curto de autoavaliação em cada revisão. O objetivo é resolver um problema universal de qualquer estudante: saber
*quais tópicos existem* é fácil, organizar *quando revisar cada um* para não esquecer é o que normalmente falta.

## Passo 1 — Entender o material

Peça (se não vier junto) a ementa/lista de tópicos, quantos dias até a prova ou objetivo final, e quantas horas por
dia a pessoa tem disponíveis para estudar. Quanto mais claro o prazo, mais preciso o cronograma.

## Passo 2 — Quebrar em tópicos estudáveis

Divida o conteúdo em unidades de estudo de 30-90 minutos cada (nem tão grandes que desanimem, nem tão pequenas que
percam contexto). Ordene por pré-requisito quando o conteúdo tiver dependência lógica (ex.: conceito base antes de
aplicação).

## Passo 3 — Montar o cronograma com revisão espaçada

Para cada tópico, gere:
- Uma sessão de **primeiro estudo**.
- Sessões de **revisão** nos intervalos padrão (1, 3, 7, 16, 35 dias após o primeiro estudo, truncando o que passar
  do prazo final).
- Um **checkpoint de autoavaliação** de 2-3 perguntas por revisão, para a pessoa testar se realmente lembra, não só
  reler.

Apresente o cronograma como uma tabela por data, com o tópico, o tipo de sessão (estudo/revisão) e o checkpoint.

## Passo 4 — Ajustar ao tempo disponível

Se o cronograma não couber no tempo/horas disponíveis informados, avise isso claramente ao usuário e priorize os
tópicos com maior peso na prova/objetivo (se ele informar pesos) em vez de simplesmente cortar em silêncio.

## Passo 5 — Entregar como arquivo

Gere o cronograma como um documento (markdown ou planilha, conforme o pedido) que o usuário possa salvar e
acompanhar, e ofereça lembrar cada bloco por scheduled task se o usuário quiser esse acompanhamento automático.

## Coisas que esta skill nunca faz

- Nunca inventa conteúdo da disciplina que não veio da ementa/material fornecido — se faltar informação, pergunta,
  não preenche com achismo.
- Nunca assume que o usuário vai seguir o cronograma à risca; se ele voltar dizendo que atrasou, reorganiza a partir
  de onde ele está, sem julgamento.
