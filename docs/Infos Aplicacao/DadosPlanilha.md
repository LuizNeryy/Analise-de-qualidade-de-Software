# DADOS PARA PREENCHIMENTO DA PLANILHA ISO/IEC 14598

Use este arquivo como guia para preencher as tabelas no arquivo Excel. As tabelas abaixo estão no mesmo formato das que você deve preencher no seu arquivo.

---

## ETAPA 1 — ESTABELECER REQUISITOS (ISO/IEC 14598)

| Campo | Descrição |
|---|---|
| Objetivo da Avaliação | Aplicar um processo estruturado de medição de qualidade de software baseado na norma ISO/IEC 14598 para diagnosticar a saúde estrutural e o desempenho de uma arquitetura baseada em microsserviços. O objetivo é identificar débitos técnicos (gargalos de processamento e anomalias de banco de dados), mensurar o impacto desses problemas na experiência final e validar, através de dados quantitativos, a eficácia de refatorações de código no reestabelecimento da qualidade do sistema. |
| Produto Avaliado | StoreDog (ecommerce-workshop) - Sistema de e-commerce desenvolvido com Spree Commerce, conteinerizado via Docker, com arquitetura de microsserviços. |
| Módulo / Escopo | Fluxo de navegação, endpoints de descontos e anúncios, integração entre frontend e serviços, observabilidade via Datadog, comparação entre cenário base (não otimizado) e cenário corrigido (pós-intervenção). |
| Características a serem avaliadas (ISO 25010) | Confiabilidade (Tolerância a falhas); Eficiência de Desempenho (Comportamento em relação ao tempo e Utilização de recursos). |

---

## ETAPA 2 — ESPECIFICAR A AVALIAÇÃO (Métricas e Critérios)

| Métrica | Escala | Critério (Meta) | Resultado (Base / Corrigido) | Status |
|---|---|---|---|---|
| Tempo de resposta (Latência) | ms | 2 | >2500 ms / 6,79 ms | NÃO / OK |
| Taxa de erro HTTP | % | 3 | ~5% / 0% | NÃO / OK |
| Queries SQL por carregamento | unidades | 2 | 208 / 42 | NÃO / OK |

| Resumo | |
|---|---|
| Total OK | 2 |
| Total NÃO | 1 |

---

## ETAPA 3 — PROJETAR A AVALIAÇÃO (Plano)

| Item | Descrição |
|---|---|
| Equipe Avaliadora | Grupo de avaliação (até 5 integrantes). Distribuição de apresentação conforme divisão de slides. |
| Método de Avaliação | Comparação controlada entre dois cenários (Cenário Base não otimizado e Cenário Corrigido pós-intervenção) usando Datadog APM, Service Map e tráfego simulado com GoReplay. |
| Ferramentas | Datadog APM, Datadog Service Map, GoReplay (tráfego simulado), PostgreSQL logs, traces distribuídos. |
| Período | Execução de 5-10 minutos por cenário para estabilizar métricas; coleta de amostras em estado estável. |

---

## ETAPA 4 — EXECUTAR A AVALIAÇÃO (Resultados)

### Cenário Base (Não otimizado) — REPROVADO

| Métrica | Resultado | Critério | Pontuação | Status |
|---|---|---|---|---|
| Tempo de resposta | >2500 ms | Meta: <100ms | 1 (Crítico) | NÃO |
| Taxa de erro HTTP | ~5% | Meta: 0% | 3 (Aceitável) | NÃO |
| Queries SQL por carregamento | 208 | Meta: <50 | 1 (Crítico) | NÃO |

**Nota Final Cenário Base:** (3×3 + 1×2 + 1×2) / 7 = 13 / 7 = **1,85**

**Decisão Final:** **NÃO APROVADO** (Descartar / Refatoração Crítica Imediata)

---

### Cenário Corrigido (Pós-Intervenção) — APROVADO

| Métrica | Resultado | Critério | Pontuação | Status |
|---|---|---|---|---|
| Tempo de resposta | 6,79 ms | Meta: <100ms | 5 (Excelente) | OK |
| Taxa de erro HTTP | 0% | Meta: 0% | 5 (Excelente) | OK |
| Queries SQL por carregamento | 42 | Meta: <50 | 4 (Bom) | OK |

**Nota Final Cenário Corrigido:** (5×3 + 5×2 + 4×2) / 7 = 33 / 7 = **4,71**

**Decisão Final:** **APROVADO COM QUALIDADE DE EXCELÊNCIA** (Manter)

---

## Observações para o Excel

- **Peso M1 (Taxa de erro):** 3 (alta criticidade)
- **Peso M2 (Latência):** 2 (média criticidade)
- **Peso M3 (Queries):** 2 (média criticidade)
- **Total de Pesos:** 7

- **Faixas de Decisão:**
  - 1,0 a 2,5: Descartar / Refatoração Crítica Imediata
  - 2,6 a 3,9: Melhorar / Aprovado com ressalvas
  - 4,0 a 5,0: Manter / Aprovado com qualidade de excelência

- **Evidências (imagens):**
  - Cenário quebrado: `04-resultados/cenario-quebrado/` (O Mapa do Problema, Erro gráfico, Latência, Spam List)
  - Cenário corrigido: `04-resultados/cenario-correto/` (O Mapa corrigido, Erro gráfico, Latência, Spam List)
