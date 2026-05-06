# Planilha ISO/IEC 14598 — Versão preenchida (Markdown)

Este arquivo reproduz as tabelas do template ISO14598 preenchidas com os
valores consolidados na avaliação. Os dados foram extraídos de:
- `04-resultados/avaliacaoInicial.md` (análise original)
- `04-resultados/resultado.md` (consolidação e complementos)
- Evidências visuais em `04-resultados/cenario-quebrado` e `cenario-correto`

Use-o como referência rápida; a planilha original em formato Excel está em
`templates/ISO14598_template-2.xlsx`.

---

## ETAPA 1 — ESTABELECER REQUISITOS (ISO/IEC 14598)

### 1.1 Definir o propósito da avaliação

Aplicar um processo estruturado de medição de qualidade de software, baseado
na norma ISO/IEC 14598, para diagnosticar a saúde estrutural e o desempenho
de uma arquitetura baseada em microsserviços. O objetivo é identificar débitos
técnicos (gargalos de processamento e anomalias de banco de dados), mensurar
o impacto desses problemas na experiência final e validar, através de dados
quantitativos, a eficácia de refatorações de código no reestabelecimento da
qualidade do sistema.

### 1.2 Identificar o software avaliado

| Campo | Descrição |
|---|---|
| **Nome do Software** | StoreDog (ecommerce-workshop) |
| **Objetivo do Software** | Operar como uma loja virtual funcional, lidando com tráfego de usuários, exibição de produtos, aplicação dinâmica de descontos e exibição de campanhas publicitárias em tempo real. |
| **Principais Funcionalidades** | • Catálogo de produtos e categorias (Frontend em Ruby on Rails)<br>• Motor de cálculo de descontos (Microsserviço em Python/Flask)<br>• Exibição de banners de anúncios (Microsserviço em Python/Flask)<br>• Integração com bancos de dados relacionais (PostgreSQL e SQLite) |
| **Público-Alvo** | Consumidores finais (shoppers) que buscam realizar compras online e times de engenharia de software/SRE focados em manter a disponibilidade. |
| **Stack Tecnológico** | Ruby on Rails (Spree), Python (Flask), PostgreSQL, SQLite, Docker, Datadog APM. |

### 1.3 Especificar o modelo de qualidade

A avaliação utilizará o modelo de qualidade de produto definido pela norma
**ISO/IEC 25010**. Dadas as características arquiteturais do software
(microsserviços), a análise foi focada nas seguintes características:

| Característica ISO/IEC 25010 | Subcaracterística | Justificativa |
|---|---|---|
| **Confiabilidade** | Tolerância a falhas | Capacidade do sistema de manter operação mesmo quando falhas de integração entre microsserviços ocorrem. |
| **Eficiência de Desempenho** | Comportamento em relação ao tempo | Tempos de resposta e latência adequados para operação do e-commerce. |
| **Eficiência de Desempenho** | Utilização de recursos | Eficiência nas consultas ao banco de dados; identificação de anomalias N+1. |

---

## ETAPA 2 — ESPECIFICAR A AVALIAÇÃO (Métricas e Critérios)

### 2.1 Definir métricas de avaliação

Foram selecionadas as características de **Confiabilidade** e **Eficiência de
Desempenho**, desdobradas nas seguintes métricas quantitativas capturadas via
APM (Datadog):

| Métrica | Característica ISO/IEC 25010 | Subcaracterística | Descrição |
|---|---|---|---|
| **M1 - Taxa de Erros HTTP** | Confiabilidade | Tolerância a Falhas | Percentual de requisições que retornam status 5xx (erros internos), avaliando a capacidade do sistema manter operação mesmo com falhas de integração. |
| **M2 - Tempo de Resposta / Latência** | Eficiência de Desempenho | Comportamento no tempo | Tempo médio em milissegundos (p50/p90) que o microsserviço leva para processar uma requisição. |
| **M3 - Volume de Consultas ao Banco de Dados** | Eficiência de Desempenho | Utilização de Recursos | Conta o número de *queries* SQL executadas por requisição para identificar anomalias como *Lazy Loading* (Query N+1). |

### 2.2 Estabelecer níveis de pontuação

Cada métrica será pontuada em uma escala de **1 a 5** (sendo 1 inaceitável e
5 excelente), com pesos baseados na criticidade para a experiência do usuário
final e operação do e-commerce:

| Métrica | Peso | Nível 5 (Excelente) | Nível 4 (Bom) | Nível 3 (Aceitável) | Nível 2 (Ruim) | Nível 1 (Crítico) | Justificativa |
|---|---:|---|---|---|---|---|---|
| **M1 - Erros HTTP** | 3 | 0% | < 2% | < 5% | < 10% | > 10% | Alta criticidade. Erros 5xx impedem vendas no e-commerce. |
| **M2 - Latência** | 2 | < 100ms | < 500ms | < 1.000ms | < 2.000ms | > 2.000ms | Média criticidade. Impacta conversão de vendas, mas não quebra o sistema. |
| **M3 - Queries SQL** | 2 | < 10 | < 50 | < 100 | < 200 | > 200 | Média criticidade. Alto volume esgota recursos do banco (N+1). |

**Total de Pesos:** 7 (utilizado para normalização da média ponderada)

### 2.3 Definir critérios de julgamento

O julgamento final será obtido pela média ponderada das pontuações das
métricas:

$$\text{Nota Final} = \frac{\sum (\text{Pontuação} \times \text{Peso})}{\sum \text{Pesos}} = \frac{\sum (\text{Pontuação} \times \text{Peso})}{7}$$

**Faixas de Decisão:**

- **1,0 a 2,5:** Descartar / Refatoração Crítica Imediata
- **2,6 a 3,9:** Melhorar / Aprovado com ressalvas
- **4,0 a 5,0:** Manter / Aprovado com qualidade de excelência

---

## ETAPA 3 — PROJETAR A AVALIAÇÃO (Plano)

### 3.1 Elaborar o plano de avaliação

A coleta de dados foi planejada utilizando a ferramenta de observabilidade
**Datadog APM**. O instrumento de coleta define a extração de evidências em
dois cenários distintos para fins de comparação prática:

| Item | Descrição |
|---|---|
| **Equipe Avaliadora** | Grupo de avaliação (até 5 integrantes). Distribuição conforme divisão de slides em `slides/Slides.md`. |
| **Método de Avaliação** | Comparação controlada entre dois cenários (base e pós-intervenção) usando Datadog APM, Service Map e tráfego simulado. |
| **Ferramentas** | Datadog APM, Datadog Service Map, GoReplay (tráfego simulado), logs de PostgreSQL. |
| **Período e Duração** | Execução de ~5 minutos por cenário para estabilizar métricas; coleta de amostras durante estado estável. Dois ciclos: Cenário Base (não otimizado) e Cenário Pós-Intervenção (corrigido). |

### 3.2 Cenários de Avaliação

| Cenário | Descrição | Evidências |
|---|---|---|
| **Cenário Base (Não otimizado)** | Sistema em estado original, sob carga de tráfego simulado (GoReplay). Contém problemas intencionais: sleeps artificiais, N+1 query, falhas de integração. | `04-resultados/cenario-quebrado/O Mapa do Problema.png`, `Erro grafico.png`, `Latencia.png`, `Spam List.png` |
| **Cenário Pós-Intervenção (Corrigido)** | Sistema após refatoração: Eager Loading ativado, sleeps removidos, integração tratada. | `04-resultados/cenario-correto/O Mapa corrigido.png`, `Erro grafico.png`, `Latencia.png`, `Spam List.png` |

---

## ETAPA 4 — EXECUTAR A AVALIAÇÃO (Resultados)

### 4.1 Coleta e tratamento dos dados

#### A. Análise de Saúde Geral da Arquitetura

O mapa de serviços indicou imediatamente as falhas de comunicação. No cenário
base, o frontend estava isolado por falhas de integração; no cenário corrigido,
a estabilidade foi restaurada.

- **Cenário Base:** [O Mapa do Problema](../04-resultados/cenario-quebrado/O%20Mapa%20do%20Problema.png)
- **Cenário Corrigido:** [O Mapa corrigido](../04-resultados/cenario-correto/O%20Mapa%20corrigido.png)

#### B. M1: Tolerância a Falhas (Confiabilidade)

Mede a taxa de respostas HTTP 5xx. No cenário base, a falha do serviço de
descontos provocou quebra em cascata no frontend, resultando em alta taxa de
falhas. Após a correção, a disponibilidade foi integralmente estabelecida.

- **Cenário Base:** [Erro gráfico](../04-resultados/cenario-quebrado/Erro%20grafico.png)
- **Cenário Corrigido:** [Erro gráfico](../04-resultados/cenario-correto/Erro%20grafico.png)

**Valores Observados:**
- Valor Esperado: 0%
- Cenário Base: ~5% (picos contínuos de erro 500)
- Cenário Corrigido: 0%

#### C. M2: Comportamento em Relação ao Tempo (Latência)

Mede a latência média das requisições. Foi identificado grave gargalo devido a
funções bloqueantes síncronas (`time.sleep`) no código legado, superando
2.5 segundos. Após refatoração, a latência desabou para milissegundos.

- **Cenário Base:** [Latência](../04-resultados/cenario-quebrado/Latencia.png)
- **Cenário Corrigido:** [Latência](../04-resultados/cenario-correto/Latencia.png)

**Valores Observados:**
- Valor Esperado: < 100ms
- Cenário Base: > 2.500ms (máximo ~2.800ms no trace)
- Cenário Corrigido: 6,79ms

#### D. M3: Utilização de Recursos (Queries SQL)

Mede o número de queries por carregamento. A análise minuciosa revelou **Query
N+1**: o uso de *Lazy Loading* (carregamento preguiçoso) do ORM gerava centenas
de conexões redundantes. Com *Eager Loading*, o consumo foi drasticamente reduzido.

- **Cenário Base:** [Spam List](../04-resultados/cenario-quebrado/Spam%20List.png)
- **Cenário Corrigido:** [Spam List](../04-resultados/cenario-correto/Spam%20List.png)

**Valores Observados:**
- Valor Esperado: < 50 queries
- Cenário Base: > 208 queries (consultadas repetidamente)
- Cenário Corrigido: 42 queries (consolidadas; redução de ~80%)

### 4.2 Comparação com Critérios

#### Resultado Consolidado — Cenário Base (Não otimizado)

| Métrica | Valor Observado | Pontuação | Peso | Contribuição |
|---|---:|---:|---:|---:|
| M1 - Erros HTTP | ~5% | 3 | 3 | 9 |
| M2 - Latência | > 2.500ms | 1 | 2 | 2 |
| M3 - Queries SQL | 208 | 1 | 2 | 2 |
| **TOTAL** | | | **7** | **13** |

**Nota final do cenário base:** 13 ÷ 7 = **1,85**

**Decisão:** Descartar / Refatoração Crítica Imediata

#### Resultado Consolidado — Cenário Corrigido (Pós-Intervenção)

| Métrica | Valor Observado | Pontuação | Peso | Contribuição |
|---|---:|---:|---:|---:|
| M1 - Erros HTTP | 0% | 5 | 3 | 15 |
| M2 - Latência | 6,79ms | 5 | 2 | 10 |
| M3 - Queries SQL | 42 | 4 | 2 | 8 |
| **TOTAL** | | | **7** | **33** |

**Nota final do cenário corrigido:** 33 ÷ 7 = **4,71**

**Decisão:** Manter / Aprovado com qualidade de excelência

### 4.3 Análise Crítica (Conclusão)

#### Pontos de Melhoria Identificados (Débito Técnico)

A arquitetura original falhava nos quesitos de resiliência e otimização:
- Comunicação entre microsserviços não tratava retornos nulos ou quebras de
  conexão (afetando diretamente confiabilidade do frontend).
- Emprego incorreto de ORM gerava antipadrão *Query N+1*, saturando recursos
  do banco PostgreSQL.
- Sleeps artificiais (2.5s) bloqueavam processamento de requisições.

#### Pontos Fortes

Apesar dos problemas no código-fonte:
- Arquitetura bem estruturada com conteinerização permitiu instrumentação
  extremamente rápida e não invasiva via APM.
- Visibilidade total dos gargalos através do Datadog Service Map e traces.
- Documentação e evidências visuais facilitaram identificação de problemas.

#### Recomendação Final

**Status:** Manter a versão corrigida (Nota 4,71)

Baseado nos critérios definidos, a recomendação formal é de **Manter** a
versão corrigida do sistema. A refatoração mitigou 100% dos erros críticos 500,
otimizou a comunicação via rede e reduziu o tempo de resposta do servidor de
2.8s para 6,79ms, atingindo patamares técnicos de excelência de mercado.
