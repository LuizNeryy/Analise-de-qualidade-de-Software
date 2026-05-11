# Relatório Técnico: Avaliação da Qualidade de Software (ISO/IEC 14598 e 25010)
**Objeto de Avaliação:** Sistema de E-commerce "StoreDog" (Arquitetura de Microsserviços)

---

## 1. Estabelecimento dos Requisitos de Avaliação

### 1.1 Definir o propósito da avaliação

O propósito desta avaliação é aplicar um processo estruturado de medição de qualidade de software, baseado na norma ISO/IEC 14598, para diagnosticar a saúde estrutural e o desempenho de uma arquitetura baseada em microsserviços. O objetivo é identificar débitos técnicos (gargalos de processamento, anomalias de banco de dados e fragilidades de segurança), mensurar o impacto desses problemas na experiência final do usuário e validar, através de dados quantitativos coletados por dois instrumentos distintos — APM em tempo real (Datadog) e análise estática de código (SonarQube) — a eficácia de refatorações no reestabelecimento da qualidade do sistema.

### 1.2 Identificar o software avaliado

O software avaliado é o **StoreDog**, um e-commerce desenvolvido com base no framework Spree Commerce e conteinerizado via Docker.

![Tela Inicial do Sistema - StoreDog](Tela%20sistema.png)

*(Visão geral da interface principal do StoreDog com a listagem de produtos e banners promocionais)*

* **Objetivo do software:** Operar como uma loja virtual funcional, lidando com tráfego de usuários, exibição de produtos, aplicação dinâmica de descontos e exibição de campanhas publicitárias em tempo real.
* **Principais funcionalidades:**
    * Catálogo de produtos e categorias (Frontend em Ruby on Rails).
    * Motor de cálculo de descontos (Microsserviço em Python/Flask).
    * Exibição de banners de anúncios (Microsserviço em Python/Flask).
    * Integração com bancos de dados relacionais (PostgreSQL e SQLite).
* **Público-alvo:** Consumidores finais (shoppers) que buscam realizar compras online e times de engenharia de software/SRE (Site Reliability Engineering) focados em manter a disponibilidade da plataforma.

### 1.3 Especificar o modelo de qualidade

A avaliação utilizará o modelo de qualidade de produto definido pela norma **ISO/IEC 25010**. Dadas as características arquiteturais do software (microsserviços com acesso a dados de usuários e transações financeiras), a análise será focada nas seguintes características e subcaracterísticas:

* **Confiabilidade:** Foco nas subcaracterísticas *Tolerância a Falhas* (capacidade do sistema de manter operação mesmo quando falhas de integração entre microsserviços ocorrem) e *Maturidade* (avaliada via bugs estáticos identificados por análise de código).
* **Eficiência de Desempenho:** Foco nas subcaracterísticas *Comportamento em relação ao tempo* (tempos de resposta e latência adequados) e *Utilização de Recursos* (eficiência nas consultas ao banco de dados).
* **Segurança:** Foco na subcaracterística *Confidencialidade e Integridade*, avaliando a presença de vulnerabilidades e pontos críticos de segurança no código-fonte — aspecto de alta criticidade em sistemas de e-commerce que manipulam dados pessoais e transações.
* **Manutenibilidade:** Foco nas subcaracterísticas *Modificabilidade* (nível de duplicação de código que dificulta alterações seguras) e *Testabilidade* (cobertura de testes automatizados que garante a detecção de regressões).

---

## 2. Especificação da Avaliação

### 2.1 Definir métricas de avaliação

Para avaliar o software sob a ótica da norma **ISO/IEC 25010**, foram selecionadas sete métricas quantitativas distribuídas entre dois instrumentos de coleta:

**Instrumento 1 — Análise Dinâmica (Datadog APM):** métricas coletadas em tempo de execução, comparando dois cenários (Base vs. Corrigido).

* **Métrica 1 (M1) - Taxa de Erros HTTP (Confiabilidade / Tolerância a Falhas):** Mede a porcentagem de requisições que retornam status HTTP 5xx (erros internos do servidor), avaliando a capacidade do sistema de manter operação mesmo com falhas de integração entre microsserviços.
* **Métrica 2 (M2) - Latência de Resposta (Eficiência de Desempenho / Comportamento no Tempo):** Mede o tempo médio em milissegundos (percentil p50) que o microsserviço de descontos leva para processar uma requisição completa, do recebimento à resposta.
* **Métrica 3 (M3) - Volume de Consultas ao Banco de Dados (Eficiência de Desempenho / Utilização de Recursos):** Conta o número de queries SQL distintas executadas pelo ORM para renderizar uma única página de produtos, com o objetivo de identificar o antipadrão *Lazy Loading* (N+1 queries).

**Instrumento 2 — Análise Estática (SonarQube):** métricas coletadas via inspeção do código-fonte, aplicadas sobre a versão corrigida do sistema.

* **Métrica 4 (M4) - Vulnerabilidades de Segurança (Segurança / Confidencialidade):** Avalia o grau de segurança do código-fonte atribuído pelo SonarQube (escala A–E), baseado no número e severidade de vulnerabilidades abertas (CWEs) identificadas na base de código.
* **Métrica 5 (M5) - Bugs Estáticos de Confiabilidade (Confiabilidade / Maturidade):** Avalia o grau de confiabilidade do código-fonte atribuído pelo SonarQube (escala A–E), baseado no número e severidade de bugs identificados por análise estática — defeitos que podem causar comportamento inesperado em produção.
* **Métrica 6 (M6) - Cobertura de Testes Automatizados (Manutenibilidade / Testabilidade):** Mede o percentual de linhas de código exercitadas por testes automatizados. Uma cobertura baixa indica alto risco de regressões silenciosas durante refatorações ou novas funcionalidades.
* **Métrica 7 (M7) - Duplicação de Código (Manutenibilidade / Modificabilidade):** Mede o percentual de blocos de código duplicados. Alta duplicação aumenta o custo de manutenção, pois correções e evoluções precisam ser replicadas em múltiplos pontos.

### 2.2 Estabelecer níveis de pontuação

Cada métrica será pontuada em uma escala de **1 a 5** (sendo 1 Crítico/inaceitável e 5 Excelente), com pesos atribuídos conforme a criticidade de cada dimensão para o contexto de um e-commerce de produção.

> **Critério de pontuação geral:** Para cada métrica, o valor medido é comparado diretamente aos limiares da tabela abaixo. O avaliador enquadra o resultado na faixa correspondente e atribui a pontuação associada. A justificativa do enquadramento deve ser explicitamente documentada na seção de resultados (4.2).

#### Métricas de Análise Dinâmica (Datadog APM)

| Métrica | Peso | Nível 5 — Excelente | Nível 4 — Bom | Nível 3 — Aceitável | Nível 2 — Ruim | Nível 1 — Crítico | Justificativa do Peso |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **M1** — Taxa de Erros HTTP | **3** | 0% | < 2% | < 5% | < 10% | ≥ 10% | Peso máximo: erros 5xx interrompem vendas e comprometem a receita diretamente. |
| **M2** — Latência de Resposta | **2** | < 100ms | < 500ms | < 1.000ms | < 2.000ms | ≥ 2.000ms | Peso médio: impacta a taxa de conversão, mas não impede a conclusão da compra. |
| **M3** — Volume de Queries | **2** | < 10 | < 50 | < 100 | < 200 | ≥ 200 | Peso médio: volumes extremos esgotam conexões do banco (N+1), degradando toda a plataforma. |

#### Métricas de Análise Estática (SonarQube)

Os graus A–E do SonarQube são convertidos diretamente para a escala de pontuação 1–5: **Grau A = 5, B = 4, C = 3, D = 2, E = 1.** Para métricas percentuais (M6 e M7), os limiares abaixo foram definidos com base nas boas práticas da indústria e nas recomendações da própria ferramenta.

| Métrica | Peso | Nível 5 — Excelente | Nível 4 — Bom | Nível 3 — Aceitável | Nível 2 — Ruim | Nível 1 — Crítico | Justificativa do Peso |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **M4** — Vulnerabilidades (Segurança) | **3** | Grau A | Grau B | Grau C | Grau D | Grau E | Peso máximo: e-commerces manipulam dados de cartão e PII; brechas de segurança geram impacto legal e financeiro severo. |
| **M5** — Bugs Estáticos (Confiabilidade) | **2** | Grau A | Grau B | Grau C | Grau D | Grau E | Peso médio: bugs estáticos representam risco latente de falhas em produção, mas não causam impacto imediato. |
| **M6** — Cobertura de Testes | **2** | ≥ 80% | ≥ 60% | ≥ 40% | ≥ 20% | < 20% | Peso médio: cobertura baixa aumenta o risco de regressões silenciosas em cada deploy. |
| **M7** — Duplicação de Código | **1** | ≤ 3% | ≤ 5% | ≤ 10% | ≤ 20% | > 20% | Peso menor: duplicação gera débito técnico de médio prazo, sem impacto imediato na operação. |

### 2.3 Definir critérios de julgamento

O julgamento final será obtido pela **média ponderada** das pontuações: `(Σ (Pontuação × Peso)) / Σ Pesos`. O resultado definirá a recomendação formal de acordo com as faixas abaixo:

| Faixa | Status | Significado |
| :---: | :--- | :--- |
| **1.0 a 2.5** | 🔴 Descartar / Refatoração Crítica Imediata | O software apresenta falhas graves que impedem seu uso seguro em produção. |
| **2.6 a 3.9** | 🟡 Melhorar — Aprovado com Ressalvas | O software é operável, mas possui débitos técnicos significativos que precisam ser endereçados. |
| **4.0 a 5.0** | 🟢 Manter — Aprovado / Qualidade de Excelência | O software atinge padrões de qualidade adequados para operação contínua. |

> **Nota metodológica:** O Cenário Base (não otimizado) será avaliado apenas com as métricas dinâmicas M1–M3, pois a análise SonarQube foi executada exclusivamente sobre a versão corrigida do código-fonte. O Cenário Corrigido será avaliado com o conjunto completo de 7 métricas (M1–M7).

---

## 3. Projeto da Avaliação

### 3.1 Elaborar o plano de avaliação

A coleta de dados foi planejada utilizando dois instrumentos complementares de observabilidade, cada um atuando em uma dimensão distinta da qualidade:

**Instrumento A — Datadog APM (Análise Dinâmica em Tempo de Execução):**
Responsável pela coleta das métricas M1, M2 e M3. Permite observar o comportamento real do sistema sob carga, capturando rastreios distribuídos (*traces*), mapas de serviço e gráficos de erros em dois momentos distintos:
1. **Cenário Base (Não Otimizado):** Sistema em seu estado original, sob carga de tráfego simulado (`traffic-replay`). Serve como linha de base para identificar os problemas.
2. **Cenário Pós-Intervenção (Corrigido):** Nova rodada de avaliação após a aplicação das correções arquiteturais (*Eager Loading* e eliminação de funções síncronas bloqueantes). Serve para validar a eficácia das mudanças.

**Instrumento B — SonarQube (Análise Estática de Código):**
Responsável pela coleta das métricas M4, M5, M6 e M7. Analisa o código-fonte do projeto em busca de vulnerabilidades de segurança, bugs estáticos, lacunas de cobertura de testes e blocos duplicados. A análise foi aplicada sobre a versão corrigida do código, com 12.000 linhas inspecionadas.

---

## 4. Execução da Avaliação

### 4.1 Coleta e tratamento dos dados

#### A. Análise de Saúde Geral da Arquitetura (Datadog)

O mapa de serviços indicou imediatamente as falhas de comunicação. No cenário base, o frontend estava isolado por falhas em cascata, e no cenário corrigido, a estabilidade foi totalmente restaurada.

![Mapa da Arquitetura demonstrando estado crítico no Frontend (Cenário Base)](cenario%20quebrado/O%20Mapa%20do%20Problema.png)
![Mapa da Arquitetura demonstrando operação saudável (Cenário Corrigido)](cenario%20correto/O%20Mapa%20corrigido.png)

#### B. M1: Tolerância a Falhas — Taxa de Erros HTTP (Confiabilidade)

No cenário base, a falha do serviço de descontos provocou uma quebra em cascata no frontend, resultando em picos contínuos de erro HTTP 500. A ausência de tratamento de exceção no código (`rescue`/`try-except`) fazia com que qualquer retorno nulo do microsserviço derrubasse a resposta da loja inteira. Após a correção, a disponibilidade foi integralmente reestabelecida.

![Gráfico evidenciando alta taxa de Erros 500 (Cenário Base)](cenario%20quebrado/Erro%20grafico.png)
![Gráfico evidenciando ausência de Erros e estabilidade (Cenário Corrigido)](cenario%20correto/Erro%20grafico.png)

| | Cenário Base | Cenário Corrigido |
| :--- | :---: | :---: |
| **Valor Medido** | ~5% (picos de erro 500) | 0% |
| **Valor Esperado** | 0% | 0% |

#### C. M2: Comportamento no Tempo — Latência de Resposta (Eficiência de Desempenho)

Foi identificado um grave gargalo de processamento causado por chamadas síncronas bloqueantes (`time.sleep`) no código legado do microsserviço de descontos, que simulavam uma integração lenta e travavam toda a thread de processamento. Após a refatoração para eliminação das funções bloqueantes, a latência caiu de mais de 2,5 segundos para a casa dos milissegundos.

![Gráfico de Latência apontando lentidão severa acima de 2.5s (Cenário Base)](cenario%20quebrado/Latencia.png)
![Gráfico de Latência evidenciando resposta otimizada em 6.79ms (Cenário Corrigido)](cenario%20correto/Latencia.png)

| | Cenário Base | Cenário Corrigido |
| :--- | :---: | :---: |
| **Valor Medido** | > 2.500ms (2,8s no topo do Trace) | 6,79ms |
| **Valor Esperado** | < 100ms | < 100ms |

#### D. M3: Utilização de Recursos — Queries ao Banco de Dados (Eficiência de Desempenho)

A análise minuciosa dos *traces* evidenciou uma clássica anomalia de arquitetura: o antipadrão **Query N+1**. O uso de *Lazy Loading* (carregamento preguiçoso) do ORM fazia com que, para cada produto exibido na listagem, uma consulta SQL adicional fosse disparada para buscar informações de desconto e categoria — gerando centenas de idas redundantes ao banco por requisição. Com a substituição por *Eager Loading*, o ORM passou a buscar todos os dados em um conjunto mínimo de consultas otimizadas.

![Lista de Spans evidenciando a anomalia N+1 com centenas de consultas repetidas (Cenário Base)](cenario%20quebrado/Spam%20List.png)
![Lista de Spans otimizada com redução drástica nas idas ao banco (Cenário Corrigido)](cenario%20correto/Spam%20List.png)

| | Cenário Base | Cenário Corrigido |
| :--- | :---: | :---: |
| **Valor Medido** | 208 queries por carregamento | 42 queries consolidadas |
| **Valor Esperado** | < 50 queries | < 50 queries |

#### E. M4–M7: Análise Estática de Código (SonarQube) — Versão Corrigida

A análise SonarQube foi executada sobre o código-fonte da versão corrigida do projeto, inspecionando um total de **12.000 linhas de código** em 14.000 linhas de escopo de duplicação. Os resultados revelaram débitos técnicos significativos que não eram visíveis na análise dinâmica.

![Dashboard SonarQube - Versão Corrigida](cenario%20correto/Dashboard%20Sonar.png)

**M4 — Vulnerabilidades de Segurança:** O SonarQube identificou **54 issues abertos** de segurança, resultando no **Grau C**. Adicionalmente, foram detectados **91 Security Hotspots** com **Grau E** — pontos críticos do código que requerem revisão manual obrigatória por potencialmente expor dados sensíveis (ex.: gestão de senhas, validação de entradas, serialização). Em um sistema de e-commerce que processa dados de pagamento, este resultado é o mais preocupante da análise estática.

**M5 — Bugs Estáticos (Confiabilidade):** Foram encontrados **122 bugs** identificados por análise estática, resultando no **Grau C**. Esses bugs representam defeitos latentes — trechos de código com comportamento incorreto ou indeterminado que podem se manifestar como falhas em produção sob condições específicas.

**M6 — Cobertura de Testes:** A cobertura de testes automatizados é de **0,0%** sobre 3.700 linhas elegíveis. Nenhum teste unitário ou de integração foi instrumentado no projeto, o que significa que qualquer refatoração futura não possui validação automática de regressão.

**M7 — Duplicação de Código:** O índice de duplicação é de **12,1%** sobre 14.000 linhas analisadas. Aproximadamente 1.700 linhas de código são duplicatas identificadas, aumentando o custo de manutenção e o risco de inconsistências quando o mesmo trecho precisar ser corrigido em múltiplos lugares.

---

### 4.2 Comparação com critérios

A seguir, cada valor medido é comparado com os limiares da tabela de pontuação (seção 2.2) para determinar a pontuação de forma explícita e rastreável.

---

#### Cenário Base (Apenas métricas dinâmicas M1–M3)

| Métrica | Valor Medido | Enquadramento e Justificativa | Pontuação |
| :--- | :---: | :--- | :---: |
| **M1** — Erros HTTP | ~5% | O valor de ~5% situa-se abaixo do limiar de 5% do Nível 3 (Aceitável), porém **em picos** ultrapassa esse limite. O enquadramento no Nível 3 é conservador e adequado dado o caráter contínuo dos picos. | **3** |
| **M2** — Latência | > 2.500ms | O valor supera 2.000ms, ultrapassando o limiar do Nível 1 (Crítico). Uma latência de 2,8 segundos está 28× acima do valor esperado de excelência (100ms) e inviabiliza a experiência de compra. | **1** |
| **M3** — Queries | 208 queries | O valor de 208 supera o limiar de 200 queries do Nível 1 (Crítico). Cada carregamento de página dispara centenas de consultas redundantes, saturando o pool de conexões do banco PostgreSQL. | **1** |

**Cálculo Final — Cenário Base:**

`((3 × 3) + (1 × 2) + (1 × 2)) / (3 + 2 + 2) = 13 / 7 = **1,85**`

> 🔴 **Status: Descartar / Refatoração Crítica Imediata** — O sistema em estado base opera abaixo do mínimo aceitável em 2 das 3 dimensões avaliadas.

---

#### Cenário Corrigido (Todas as métricas M1–M7)

| Métrica | Valor Medido | Enquadramento e Justificativa | Pontuação |
| :--- | :---: | :--- | :---: |
| **M1** — Erros HTTP | 0% | O valor de 0% atinge exatamente o limiar do Nível 5 (Excelente). Nenhuma requisição retornou erro 5xx no período monitorado após a correção. | **5** |
| **M2** — Latência | 6,79ms | O valor de 6,79ms encontra-se muito abaixo do limiar de 100ms do Nível 5 (Excelente), representando uma melhora de 99,8% em relação ao cenário base. | **5** |
| **M3** — Queries | 42 queries | O valor de 42 queries situa-se entre 10 e 50, enquadrando-se no Nível 4 (Bom). A meta de excelência (< 10) não foi atingida, indicando possibilidade de otimização adicional. | **4** |
| **M4** — Vulnerabilidades | Grau C | O Grau C corresponde diretamente à pontuação 3 (Aceitável). Com 54 vulnerabilidades abertas e 91 hotspots com Grau E, o sistema apresenta riscos de segurança relevantes para um e-commerce. | **3** |
| **M5** — Bugs Estáticos | Grau C | O Grau C corresponde diretamente à pontuação 3 (Aceitável). Os 122 bugs identificados representam risco latente de falhas em produção sob condições específicas. | **3** |
| **M6** — Cobertura | 0,0% | O valor de 0% está abaixo do limiar de 20% do Nível 1 (Crítico). A ausência total de testes automatizados significa que qualquer alteração no código não possui validação de regressão. | **1** |
| **M7** — Duplicação | 12,1% | O valor de 12,1% situa-se entre 10% e 20%, enquadrando-se no Nível 2 (Ruim). Aproximadamente 1.700 linhas duplicadas elevam o custo de manutenção e risco de inconsistências. | **2** |

**Cálculo Final — Cenário Corrigido:**

`((5 × 3) + (5 × 2) + (4 × 2) + (3 × 3) + (3 × 2) + (1 × 2) + (2 × 1)) / (3 + 2 + 2 + 3 + 2 + 2 + 1)`

`= (15 + 10 + 8 + 9 + 6 + 2 + 2) / 15 = 52 / 15 = **3,47**`

> 🟡 **Status: Melhorar — Aprovado com Ressalvas** — O sistema corrigido é operacionalmente estável, mas apresenta débitos técnicos significativos nas dimensões de segurança e manutenibilidade.

---

#### Quadro Resumo Comparativo

| Métrica | Peso | Pontuação Base | Pontuação Corrigida | Variação |
| :--- | :---: | :---: | :---: | :---: |
| **M1** — Taxa de Erros | 3 | 3 | 5 | +2 ✅ |
| **M2** — Latência | 2 | 1 | 5 | +4 ✅ |
| **M3** — Queries | 2 | 1 | 4 | +3 ✅ |
| **M4** — Segurança (SonarQube) | 3 | N/A | 3 | — |
| **M5** — Bugs Estáticos | 2 | N/A | 3 | — |
| **M6** — Cobertura de Testes | 2 | N/A | 1 | — |
| **M7** — Duplicação | 1 | N/A | 2 | — |
| **Média Ponderada Final** | — | **1,85** | **3,47** | **+1,62** |

---

### 4.3 Análise Crítica (Conclusão)

A avaliação de qualidade fundamentada nas normas ISO/IEC 14598 e ISO/IEC 25010, realizada com dois instrumentos complementares (Datadog APM e SonarQube), revelou um panorama mais completo e matizado do que a análise dinâmica isolada indicaria.

**Pontos de Melhoria Identificados (Débito Técnico):**

A arquitetura original falhava criticamente nos quesitos de resiliência e eficiência de desempenho — problemas endereçados com sucesso pelas refatorações aplicadas. Contudo, a análise estática do SonarQube expôs débitos técnicos preexistentes que permanecem na versão corrigida:

* **Segurança (M4):** O Grau C em vulnerabilidades (54 issues) e o Grau E em Security Hotspots (91 pontos críticos) indicam que o código-fonte não foi desenvolvido com práticas de *Secure Coding* (ex.: validação de entradas, proteção contra injeção, gestão segura de credenciais). Para um e-commerce, este é o ponto de maior risco do sistema, com potencial de impacto legal (LGPD) e financeiro.
* **Testabilidade (M6):** A cobertura de testes de 0,0% é crítica. Toda a melhoria de desempenho alcançada carece de uma rede de segurança automatizada. Qualquer nova funcionalidade ou refatoração futura é de alto risco, pois não há mecanismo de detecção de regressões.
* **Modificabilidade (M7):** Os 12,1% de duplicação indicam que o código cresceu de forma não estruturada, com soluções copiadas ao invés de abstraídas. Isso eleva o custo de manutenção futuro.

**Pontos Fortes:**

A decisão arquitetural de conteinerizar o sistema via Docker permitiu uma instrumentação extremamente rápida e não invasiva via Datadog APM, garantindo visibilidade total dos gargalos em tempo de execução. A refatoração aplicada foi cirúrgica e eficaz: eliminou 100% dos erros 5xx, reduziu a latência de 2.800ms para 6,79ms (melhora de 99,8%) e diminuiu o volume de queries de 208 para 42 (redução de 80%). Adicionalmente, o Grau A em Manutenibilidade do SonarQube (269 issues, mas classificados como de baixa severidade) indica que o código, apesar dos problemas, é estruturalmente legível e organizado.

**Recomendação:**

Com base nos critérios definidos, a média ponderada final do sistema corrigido é de **3,47**, enquadrando-se na faixa **"Melhorar — Aprovado com Ressalvas"**. A recomendação formal para a engenharia é de **manter** a versão corrigida em operação — dado que os problemas críticos de desempenho e confiabilidade foram sanados — mas com um plano de melhoria estruturado nas seguintes prioridades:

1. **Alta prioridade:** Realizar auditoria e correção dos Security Hotspots (Grau E) e vulnerabilidades abertas, priorizando as de severidade alta/crítica, em conformidade com a LGPD.
2. **Média prioridade:** Implementar cobertura de testes automatizados progressiva, com meta inicial de ≥ 40% (Nível 3 — Aceitável) para as rotas críticas de negócio (checkout, cálculo de descontos).
3. **Baixa prioridade:** Refatorar os blocos de código duplicados, consolidando lógicas repetidas em módulos reutilizáveis para reduzir o índice de duplicação para abaixo de 5% (Nível 4 — Bom).