# Plano de Avaliação — Instrumento e Checklist

## Equipe Avaliadora

- Equipe: Grupo de avaliação (até 5 integrantes)
- Responsáveis: divisão por slides/tópicos conforme `slides/Slides.md`

## Método de Avaliação

- Abordagem: comparação entre dois cenários (quebrado vs corrigido) usando
  métricas instrumentadas e evidências visuais.
- Coleta: uso de Datadog APM/RUM para latência e erros, logs para rastrear
  exceções, e GoReplay para gerar carga controlada.

## Ferramentas

- Datadog (APM, RUM, Service Map).
- GoReplay para replay de tráfego.
- PostgreSQL (logs de queries) para contagem de consultas.
- Ferramentas locais: `curl`, `ab`/`wrk` para cargas rápidas quando necessário.

## Período

- Execução dos testes: ambiente local/docker-compose durante a janela de
  experimentação (sugestão: 10 minutos por cenário para estabilidade dos
  sinais); registrar timestamps das coletas.

## Instrumento (Checklist e Procedimento de Medição)

Preencha uma linha por execução/variante do cenário. As medições podem ser
obtidas a partir de dashboards do Datadog e a partir dos logs/traces.

| Métrica | Descrição | Unidade | Procedimento de medição | Fonte / Ferramenta |
|---|---|---:|---|---|
| M1 - Taxa de erros HTTP | Percentual de requisições 5xx sobre total | % | Calcular (5xx / total) * 100 em janela estável (últimos 5 minutos) | Datadog APM / Logs |
| M2 - Latência média | Tempo médio de resposta das requisições | ms | Média aritmética das latências das requisições relevantes (página inicial / endpoints críticos) | Datadog APM |
| M3 - Queries SQL por carregamento | Número de queries executadas por carregamento de página | unidades | Contar queries (traces ou logs de DB) correlacionadas a uma carga típica de navegação | Traces/Logs / PostgreSQL |

### Escalas e Conversão para Pontuação (1..5)

As pontuações seguem a tabela usada no relatório consolidado (ver
`04-resultados/resultado.md`). Procedimento rápido para atribuição:

- Para cada métrica, converter o valor observado para a faixa 1..5 segundo
  os limiares definidos no relatório. Registrar a pontuação e multiplicar pelo
  peso associado.

### Checklist de execução

1. Garantir que o ambiente do cenário (quebrado/corrigido) esteja em estado desejado.
2. Iniciar coleta no Datadog e zerar janelas temporais.
3. Gerar tráfego com GoReplay (configurar replay para 2-5 minutos de execução contínua).
4. Registrar os valores de M1, M2 e M3 conforme tabelas acima.
5. Preencher a planilha (ou `docs/planilha.md`) com os valores e status (OK / NÃO).
6. Anexar capturas de tela das métricas no Datadog em `04-resultados/`.

## Critérios de aceitação

- Nota final é média ponderada conforme fórmula:

$$\text{Nota Final} = \frac{\sum (Pontuação_i \times Peso_i)}{\sum Peso_i}$$

- Faixas de decisão:
  - 1,0 a 2,5: Descartar / refatoração crítica imediata
  - 2,6 a 3,9: Melhorar / aprovado com ressalvas
  - 4,0 a 5,0: Manter / aprovado com qualidade de excelência

## Observações

- Documentar data/hora e logs de execução para rastreabilidade.
- Se houver discrepância entre fontes (ex.: Datadog vs contagem em DB),
  favor registrar ambas e justificar a escolha da medida usada.
