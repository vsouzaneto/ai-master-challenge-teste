# Process Log — Challenge 001 · Diagnóstico de Churn (RavenStack)

Registro macro do trabalho feito em conversa com o Claude (IA da Anthropic, com execução de código): quantas interações houve, quanto tempo levaram, que ajustes eu pedi, onde intervim e como o projeto foi guiado.

---

## 1. Visão geral

| Indicador | Valor |
|---|---|
| Pedidos meus (prompts) | **22** |
| Dias de trabalho | 2 (21/10/2026 e 22/10/2026) |
| Tempo com horário registrado | Dia 1: 16:14 → ~18:30 (~2h15). Dia 2: 10:27 → 13:04 (~2h40). **Total registrado: ~4h55** |
| Tempo sem registro | Os 4 primeiros pedidos do dia 1 (roteiro, Fases 1 e 2, checklist e primeiro painel) não têm horário |
| Fases do roteiro | Planejadas 8 (0 a 8) → entregues **11** (0 a 10) |
| Pedidos de ajuste ou correção feitos por mim | **6** (ver seção 4) |
| Erros da IA corrigidos | **10**, mais 1 achado novo durante a revisão (ver seção 5) |
| Entregáveis no repositório | 7 scripts Python, 1 SQL, 15 arquivos de saída, 4 documentos, 5 CSVs de dados |
| Versões do painel de diagnóstico | 7 publicações, com 21 fontes de dados ligadas aos números |

### Pedidos por tipo

| Tipo | Quantidade | Pedidos |
|---|---|---|
| Planejamento e guia | 2 | #1, #12 |
| Construção (dados, SQL, painéis, modelo, ferramenta) | 8 | #2, #4, #6, #11, #17, #19, #20, #21 |
| Perguntas de negócio e hipóteses minhas | 7 | #3, #5, #7, #8, #9, #10, #14 |
| Ajustes e correções pedidos por mim | 5 | #13, #15, #16, #18, #22 (este log) |

---

## 2. Linha do tempo

| # | Quando | Meu pedido (resumo) | Tipo | O que saiu |
|---|---|---|---|---|
| 1 | Dia 1 (sem horário) | Passo a passo para entregar o desafio | Planejamento | Roteiro de 8 fases com tempos, checklist e divisão de horas |
| 2 | Dia 1 (sem horário) | Fases 1 e 2: entender os dados e montar a base mestre para subir no Lovable/Supabase | Construção | Validação das 5 tabelas, problemas de qualidade, `accounts_master.csv`, SQL, dicionário de dados |
| 3 | Dia 1 (sem horário) | Checklist: explicar o problema, mostrar o cruzamento das 5 tabelas e um número verificável em cada insight | Pergunta de negócio | Relatório de diagnóstico e `evidence.csv` com 32 números, fonte e teste |
| 4 | Dia 1 (sem horário) | Painel para o CEO: o que se pode e o que não se pode afirmar | Construção | `dashboard_ceo_churn.html` |
| 5 | 21/10 16:14 | Se o churn se mantém, é saturação natural da base? | Pergunta de negócio | Resposta: os dados não sustentam a tese de saturação |
| 6 | 21/10 16:24 | Validar a fala do CEO, buscar causas e gerar a tabela mestre em CSV com o SQL | Construção | SQL testado em PostgreSQL 16; **erro da taxa de churn descoberto (43,1% → 32,2%)** |
| 7 | 21/10 16:37 | Há degradação no tempo de uso de alguma feature? | Pergunta de negócio | Teste por feature com correção para 40 testes: sem degradação significativa |
| 8 | 21/10 16:43 | Algum canal de aquisição tem mais churn (aquisição sem foco)? | Pergunta de negócio | Resultado contrário à hipótese: **ads retém mais** (p = 0,005) |
| 9 | 21/10 16:54 | Tickets de quem saiu × quem ficou, uso por segmento e impacto financeiro | Pergunta de negócio | **Descoberta: 53% do uso é anterior ao cadastro**; MRR perdido de US$ 2,91 mi |
| 10 | 21/10 17:29 | Por que as assinaturas se sobrepõem? Trial e situação no dia 30 | Pergunta de negócio | Assinaturas nunca encerradas; trial não funciona como funil; status no dia 30 |
| 11 | 21/10 17:52 | Acrescentar essas colunas à base | Construção | 8 colunas novas no SQL e no CSV (106 colunas) |
| 12 | 21/10 18:19 | Painel com todas as fases do roteiro, SQL e arquivos | Planejamento | Painel de diagnóstico fase a fase (Fases 0 a 8) |
| 13 | 22/10 10:27 | Atualizar relatório, painel, evidências, série mensal e dicionário com os números corrigidos | Ajuste | 5 documentos atualizados; lista de risco refeita (19 → 17 contas) |
| 14 | 22/10 10:44 | Nova fase: correlação × causalidade e se o CEO consegue ler e agir | Pergunta de negócio | Fase 9 com escada de evidência e conclusões para o CEO |
| 15 | 22/10 10:49 | Remover tudo que não tenha base nos dados | Ajuste | Metas, prazos, donos e piloto removidos do painel |
| 16 | 22/10 10:52 | Aplicar o mesmo critério ao relatório e ao painel executivo | Ajuste | Os dois documentos revisados |
| 17 | 22/10 10:54 | Criar um modelo preditivo de churn | Construção | Modelo com fotos mensais e validação temporal: **só a idade da conta prevê (AUC 0,645)** |
| 18 | 22/10 11:07 | Colocar o modelo no painel e em todos os documentos | Ajuste | Fase 6 do painel, relatório e painel executivo atualizados |
| 19 | 22/10 11:14 | Ferramenta de automação para o CS, ligada aos tickets, numa nova fase | Construção | `cs_risk_tool.py` e Fase 10 com fila, regras e ficha por conta |
| 20 | 22/10 11:31 | Documento no template de submissão | Construção | `SUBMISSION.md` |
| 21 | 22/10 12:47 | Organizar os arquivos em pastas para o GitHub | Construção | Repositório testado do zero; **achado novo: erro da feature_9 dobrou** |
| 22 | 22/10 13:04 | Log macro do processo | Ajuste | Este documento |

---

## 3. Como guiei o projeto

| Guia | Como foi aplicado |
|---|---|
| **Roteiro em fases** | Pedi o roteiro primeiro (#1) e segui fase a fase. Ao fim, ele cresceu de 8 para 11 fases, com causa e decisão (Fase 9) e a ferramenta de CS (Fase 10). |
| **Critérios do desafio como checklist** | Usei os critérios de qualidade como pedidos diretos: cruzar as 5 tabelas e ter número verificável (#3), separar correlação de causalidade e ser legível para o CEO (#14). |
| **Destino prático desde o início** | Defini que a base tinha que subir no Lovable/Supabase (#2, #6) e que o diagnóstico tinha que virar uma ferramenta que o CS usaria (#19). |
| **Hipóteses de negócio próprias** | Levantei as perguntas que guiaram a investigação: saturação, degradação de features, canal sem foco, tickets, uso por segmento, peso financeiro, trial e dia 30 (#5, #7, #8, #9, #10). |
| **Só o que tem base nos dados** | Proibi recomendações sem um número que as sustente (#15, #16). |
| **Consistência entre documentos** | Exigi que cada correção chegasse a todos os entregáveis, não só ao último arquivo (#13, #18). |

---

## 4. Ajustes que pedi nos prompts

| # | Ajuste pedido | Por quê | Efeito |
|---|---|---|---|
| 11 | Acrescentar as colunas do dia 30 e de trial à base | Levar a análise para a tabela usada no Lovable | Tabela mestre passou de 98 para 106 colunas |
| 13 | Atualizar todos os documentos com o uso válido e a taxa corrigida | Os documentos ainda traziam números já invalidados | 5 documentos corrigidos; lista de risco e códigos de evidência refeitos |
| 15 | Remover sugestões sem base nos dados | Recomendação sem número não se sustenta diante do CEO | Metas (99%, 85%), prazos, donos e piloto saíram do painel |
| 16 | Aplicar o mesmo critério ao relatório e ao painel executivo | Coerência entre entregáveis | Os dois documentos revisados |
| 18 | Levar o modelo ao painel e a todos os documentos | O modelo mudou uma conclusão anterior ("nada prevê churn") | Fase 6, Fases 7 e 9, relatório e painel executivo atualizados |
| 22 | Gerar este log | Exigência de process log da submissão | Este documento |

---

## 5. Erros da IA e correções

| # | Onde | Erro | Antes → depois | Quem percebeu |
|---|---|---|---|---|
| 1 | Taxa de churn | Contas novas entravam no numerador, mas não na base | 43,1% → 32,2% no 4T24 | IA, ao validar em SQL no pedido #6 |
| 2 | Uso | "Uso cresceu" somava o uso anterior ao cadastro | +2,6% total → +2,3% por conta ativa | IA, ao responder meu pedido #9 |
| 3 | Modelo transversal | Sinal de uso vinha do tempo de vida da conta | AUC 0,58–0,60 → 0,47 | IA, ao montar o painel (#12) |
| 4 | Features | Falso positivo de múltiplos testes | 4 → 0 features com duração degradada | IA, com correção de Benjamini-Hochberg |
| 5 | Lista de risco | Queda de uso calculada com uso inválido | 19 contas / US$ 540 mil → 17 / US$ 394 mil | IA, no meu pedido de atualização (#13) |
| 6 | Relatório | Dizia que nenhum canal se destaca | Corrigido: ads retém mais | IA, no meu pedido de atualização (#13) |
| 7 | SQL | Tipo da nota de satisfação, data com hora, coluna vazia no dia 30 | Corrigidos | IA, rodando no PostgreSQL |
| 8 | Modelo | Foto única tratava a idade como vazamento | Fotos mensais: AUC 0,645 | IA, no pedido #17 |
| 9 | Ferramenta de CS | Regra de renovação automática acionava 112 de 148 contas | Critério relativo: 37 contas | IA, no pedido #19 |
| 10 | Painel | Metas, donos, prazos e piloto sem base nos dados | Removidos | **Eu**, nos pedidos #15 e #16 |
| — | Achado novo | Análise por feature refeita com uso válido | Erro da feature_9 dobrou (3,6 → 7,1 por 100 usos, q = 0,03) | IA, ao organizar o repositório (#21) |

---

## 6. Como o projeto ficou organizado

| Fase | Entregável | Arquivo |
|---|---|---|
| 0 · Setup | Roteiro e mapa de arquivos | `README.md`, painel (Fase 0) |
| 1 · Dados | Qualidade e dicionário | `docs/data_dictionary_and_quality.md` |
| 2 · Tabela mestre | Base para Supabase/Lovable | `sql/build_accounts_master.sql`, `outputs/accounts_master.csv` |
| 3 · Contradições | Evidências, série mensal, features, canal | `src/evidence.py`, `src/build_monthly_trend.py`, `src/feature_channel_analysis.py` |
| 4 · Causalidade | Hipóteses testadas | `outputs/evidence.csv`, painel (Fase 4) |
| 5 · Valor | Impacto em MRR e contas em risco | `outputs/at_risk_accounts.csv` |
| 6 · Diferenciais | Modelo preditivo e previsões | `src/churn_model.py`, `outputs/model_*.csv` |
| 7 · Relatório | Relatório e painel executivo | `docs/diagnostico_churn_ravenstack.md`, `docs/dashboard_ceo_churn.html` |
| 8 · Process log | Correções e workflow | Este documento, painel (Fase 8), `SUBMISSION.md` |
| 9 · Causa e decisão | Escada de evidência e conclusões | Painel (Fase 9) |
| 10 · Ferramenta de CS | Fila, regras e ficha por conta | `src/cs_risk_tool.py`, `outputs/cs_*.csv`, painel (Fase 10) |

**Painel de diagnóstico fase a fase:** https://claude.ai/artifact/HNmaQcLqcSEXyxg6wfyKgA

**Validação final:** o repositório foi extraído numa pasta limpa e o pipeline rodou do zero com `run_all.sh`. As 15 saídas foram idênticas às entregues.
