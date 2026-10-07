---
name: reference-analysis
description: Analisa as referências bibliográficas de um texto acadêmico (artigo, resumo expandido, TCC, dissertação etc.) SEMPRE em conjunto com o texto-base que as cita. Use esta skill sempre que o usuário pedir para "analisar as referências", "dizer quais referências são úteis para entender o trabalho", "ver o que falta para o artigo ficar pronto", "preparar pauta de reunião de alinhamento" sobre um texto acadêmico, "avaliar se uma referência/livro/autor cabe no trabalho", ou pedir o link/fonte de uma referência citada. Também ative quando o usuário colar um artigo/resumo expandido e pedir uma leitura crítica do estado do trabalho e da literatura usada. Esta skill NUNCA analisa uma lista de referências isolada sem o texto-base — sempre cruza referência com o papel que ela exerce dentro do argumento do texto.
---

# Reference Analysis

Esta skill estrutura como analisar as referências bibliográficas de um texto acadêmico **sempre em conjunto com o texto-base** (o artigo, resumo expandido, projeto, TCC etc. que cita essas referências). A referência nunca é avaliada isoladamente — o que importa é o papel que ela exerce dentro do argumento do texto-base.

Use esta skill quando o usuário:
- Pedir para identificar quais referências são essenciais para entender o trabalho
- - Pedir o que falta para o artigo/texto ficar pronto (gaps metodológicos, teóricos, de dados)
  - - Estiver se preparando para uma reunião de alinhamento/orientação e precisar de pauta
    - - Pedir o link/fonte de uma referência específica citada no texto
      - - Perguntar se um livro/autor/referência nova "cabe" ou "convém" ser adicionado ao trabalho
       
        - ## Processo
       
        - ### 1. Ler o texto-base com atenção structural
       
        - Antes de tocar nas referências, mapeie a estrutura do texto-base:
        - - Qual é a pergunta/hipótese central?
          - - Quais são as seções (introdução, métodos, resultados, considerações finais)?
            - - Quais variáveis, dados e fontes empíricas sustentam os números citados?
              - - Qual o estágio do texto: ideia inicial, resumo expandido, working paper, artigo completo, versão final?
               
                - Sinais de que o texto está incompleto (e que vale procurar): menções a análises "preliminares", "versões em desenvolvimento incorporarão...", resultados sem tabelas/figuras correspondentes, métodos descritos mas não executados.
               
                - ### 2. Classificar cada referência por papel funcional
               
                - Não trate as referências como uma lista plana. Categorize cada uma segundo a função que exerce no texto-base:
               
                - - **Fonte de dados empíricos** — de onde vêm os números, variáveis, estatísticas citadas no texto. Geralmente são notas técnicas, relatórios oficiais, bases de dados. Sem entender essas, não se entende de onde vêm as variáveis do estudo.
                  - - **Arcabouço teórico estruturante** — modelos/conceitos citados na introdução E retomados nas considerações finais; são o "fio condutor" do argumento. Geralmente poucas (1-3), mas indispensáveis.
                    - - **Apoio complementar/contextual** — dá suporte a um ponto específico (causa física de um fenômeno, paralelo internacional, dado regional de contexto) mas não é indispensável para entender a espinha dorsal do argumento.
                      - - **Referência decorativa ou não utilizada** — aparece na lista de referências mas não é de fato citada/usada no corpo do texto, ou é citação solta sem conexão clara com o argumento. Vale sinalizar como possível erro/sobra a ser cortada.
                       
                        - Ao apresentar ao usuário, sempre explique o PORQUÊ de cada referência ser essencial ou complementar — não basta listar, é preciso justificar a partir do papel que ela cumpre no argumento.
                       
                        - ### 3. Verificar externamente quando necessário
                       
                        - Se uma referência, dado ou afirmação do texto-base parecer precisar de verificação (números, existência da fonte, atualidade do dado, ou se o usuário pedir o link/fonte de algo citado), use web_search/web_fetch para confirmar antes de responder. Não invente links ou DOIs.
                       
                        - Para localizar links de fontes acadêmicas:
                        - - Notas técnicas de institutos de pesquisa (IPEA, IBGE etc.) geralmente têm página de repositório institucional — buscar pelo título exato + nome do instituto.
                          - - Livros comerciais (editoras como Routledge, Cultrix etc.) não têm PDF gratuito legítimo na maioria dos casos — ofereça a página oficial da editora, Google Books, ou sugira verificar acesso institucional (CAPES, bibliotecas universitárias) em vez de indicar PDFs piratas/scribd/sites não oficiais.
                            - - Se o link direto falhar ou não carregar, ofereça ao usuário uma string de busca pronta para colar no Google em vez de insistir no mesmo link.
                             
                              - ### 4. Avaliar pedidos de nova referência ("isso cabe no trabalho?")
                             
                              - Quando o usuário perguntar se vale adicionar uma referência nova (livro, autor, conceito):
                             
                              - 1. Identifique a escola/corrente teórica de origem da referência proposta.
                                2. 2. Compare com a escola/corrente já adotada no texto-base.
                                   3. 3. Verifique coerência: a referência nova reforça, complementa ou **contradiz** o enquadramento teórico já em uso?
                                      4. 4. Se houver contradição ou tensão teórica (ex.: uma obra de eficiência de mercado/eco-eficiência empresarial vs. um texto fundamentado em economia política ecológica/justiça ambiental, que tipicamente critica esse tipo de abordagem), explique a incompatibilidade claramente e desaconselhe — não baseie a recomendação só no tema ("ambos falam de meio ambiente"), mas na lógica argumentativa de cada corrente.
                                         5. 5. Se for compatível, sugira opcionalmente autores/obras da mesma linha já adotada que reforçariam a fundamentação sem introduzir ruído teórico.
                                           
                                            6. ### 5. Montar a saída
                                           
                                            7. Estruture a resposta em três blocos, sempre nesta ordem:
                                           
                                            8. **A. Referências essenciais para entender o trabalho** — separadas por papel funcional (dados empíricos vs. arcabouço teórico), com justificativa de cada uma.
                                           
                                            9. **B. O que falta para o texto ficar pronto** — gaps concretos identificados a partir dos próprios sinais do texto (análises prometidas mas não feitas, tabelas/figuras ausentes, testes de robustez pendentes, revisão de literatura rasa, dados ainda não disponíveis, referências citadas na bibliografia mas não usadas no corpo do texto).
                                           
                                            10. **C. Pauta sugerida** (quando o contexto for reunião de alinhamento/orientação) — lista objetiva de decisões e responsabilidades a definir, não apenas tarefas técnicas.
                                           
                                            11. Mantenha tom direto e prático — quem está pedindo isso geralmente está sob pressão de prazo (reunião no dia seguinte, entrega próxima) e quer uma leitura rápida e acionável, não um ensaio.
                                            12. 
