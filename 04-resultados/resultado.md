# Seminário Medição da Qualidade de Software

## Relatório Técnico de Avaliação da Qualidade de Software

**Software avaliado:** ecommerce-workshop / StoreDog

**Normas de referência:** ISO/IEC 14598 e ISO/IEC 25010

**Escopo:** relatório consolidado para apoiar apresentação em sala e entrega técnica.

---

## 1. Estabelecimento dos Requisitos de Avaliação

### 1.1 Definir o propósito da avaliação

O objetivo desta avaliação é aplicar, na prática, um processo estruturado de medição de qualidade de software com base na ISO/IEC 14598, utilizando o modelo de qualidade da ISO/IEC 25010 para examinar o ecommerce-workshop em um contexto de e-commerce distribuído. A avaliação busca verificar se o sistema apresenta condições aceitáveis de uso, estabilidade e desempenho para um cenário de operação contínua, considerando falhas intencionais presentes no código, integração entre serviços e impacto na experiência do usuário.

Esta análise tem caráter acadêmico e prático. Em termos acadêmicos, permite demonstrar a aplicação de métricas de qualidade, pesos e critérios de julgamento sobre um software real com acesso ao código-fonte. Em termos práticos, evidencia quais falhas afetam o sistema, quais pontos foram corrigidos e se a versão corrigida pode ser considerada mais adequada para manutenção e evolução.

### 1.2 Identificar o software avaliado

#### Nome do software

ecommerce-workshop, também apresentado no material do projeto como StoreDog.

#### Descrição geral

O ecommerce-workshop é um repositório de laboratório criado para demonstrar observabilidade em um sistema de e-commerce. A aplicação principal é um storefront baseado em Spree/Rails, complementado por microserviços em Flask para descontos e anúncios, além de um frontend auxiliar em React/Vite para interação com descontos e banners. O projeto inclui instrumentação com Datadog para APM, logs, RUM, synthetics e serviço de catálogo.

O sistema não foi projetado como produto comercial final, mas como cenário de estudo. O repositório contém versões quebradas e corrigidas dos serviços, permitindo comparar comportamento, diagnosticar falhas e medir o efeito das correções.

#### Objetivo do software

O ecommerce-workshop é um ambiente didático de e-commerce desenvolvido para demonstrar práticas de observabilidade, diagnóstico de incidentes, análise de desempenho e comparação entre versões de deploy. Permite que usuários naveguem por produtos, consultem descontos, visualizem anúncios e acompanhem o comportamento do sistema por meio de telemetria coletada no Datadog.

#### Principais funcionalidades

- Navegação de produtos no storefront principal.
- Fluxo de checkout e atualização de pedidos.
- Consulta de descontos por serviço dedicado.
- Exibição de anúncios e banners em múltiplas versões.
- Listagem, ordenação e salvamento local de descontos no navegador.
- Troca manual de anúncios no frontend React.
- Instrumentação de APM, logs, RUM e synthetics.
- Replay de tráfego para geração de carga e observabilidade.
- Comparação de versões de serviços para análise de deploy.

#### Público-alvo

O software é direcionado a estudantes, docentes e equipes que desejam praticar avaliação de qualidade de software, observabilidade e análise de sistemas distribuídos em um cenário de e-commerce.

#### Contexto técnico

- Stack: Ruby on Rails/Spree, Python Flask, React 17, TypeScript, Vite, PostgreSQL, SQLite e Datadog.
- Tipo: aplicação web distribuída.
- Número de usuários esperados: baixo a médio, suficiente para uso acadêmico e demonstração.
- Criticidade: média, pois o sistema simula fluxo de compra, anúncio e desconto com impacto direto na experiência do usuário.

### 1.3 Especificar o modelo de qualidade

As características da ISO/IEC 25010 selecionadas para este trabalho foram escolhidas por serem diretamente relevantes a um e-commerce. O sistema precisa funcionar corretamente, manter-se disponível, responder rapidamente, ser utilizável e permitir manutenção e portabilidade.

Características avaliadas:

- Adequação Funcional
- Confiabilidade
- Desempenho
- Usabilidade
- Compatibilidade
- Segurança
- Manutenibilidade
- Transferibilidade

---

## 2. Especificação da Avaliação

### 2.1 Definir métricas de avaliação

Foram definidas três métricas principais, derivadas das características mais críticas para o cenário analisado.

| Métrica | Característica ISO/IEC 25010 | Subcaracterística | Descrição |
|---|---|---|---|
| M1 | Confiabilidade | Tolerância a falhas | Mede a taxa de respostas 5xx e a capacidade de manter o serviço operando diante de falhas de integração. |
| M2 | Desempenho | Comportamento em relação ao tempo | Mede a latência média das requisições e o tempo necessário para processar a resposta. |
| M3 | Desempenho | Utilização de recursos | Mede o volume de consultas ao banco de dados por requisição, com foco em identificar N+1 query. |

### 2.2 Estabelecer níveis de pontuação

Cada métrica foi convertida para uma escala de 1 a 5, com pesos definidos de acordo com a criticidade para o contexto de e-commerce.

| Métrica | Peso | Nível 5 | Nível 4 | Nível 3 | Nível 2 | Nível 1 | Justificativa |
|---|---:|---|---|---|---|---|---|
| M1 - Erros HTTP | 3 | 0% | < 2% | < 5% | < 10% | > 10% | Erros 5xx afetam diretamente vendas e disponibilidade. |
| M2 - Latência | 2 | < 100ms | < 500ms | < 1.000ms | < 2.000ms | > 2.000ms | Lentidão reduz conversão e piora a experiência de navegação. |
| M3 - Queries SQL | 2 | < 10 | < 50 | < 100 | < 200 | > 200 | Muitos acessos ao banco indicam desperdício de recursos e N+1. |

### 2.3 Definir critérios de julgamento

O julgamento final utiliza média ponderada:

$$
\text{Nota Final} = \frac{\sum (\text{Pontuação} \times \text{Peso})}{\sum \text{Pesos}}
$$

Faixas de decisão adotadas:

- 1,0 a 2,5: Descartar / refatoração crítica imediata.
- 2,6 a 3,9: Melhorar / aprovado com ressalvas.
- 4,0 a 5,0: Manter / aprovado com qualidade de excelência.

---

## 3. Projeto da Avaliação

### 3.1 Elaborar o plano de avaliação

O instrumento de avaliação foi planejado a partir da observabilidade disponível no projeto e do comportamento observado no código. A coleta de dados foi feita em dois cenários: cenário base, com os problemas intencionais presentes, e cenário corrigido, com as versões ajustadas dos serviços.

#### Instrumentos utilizados

- Datadog APM para coleta de traces, latência e erros.
- Datadog Service Map para análise da arquitetura.
- GoReplay para gerar tráfego de teste.
- Frontend React com ações instrumentadas para observabilidade de comportamento.

#### Itens do checklist de avaliação

- As funcionalidades principais estão implementadas.
- Os endpoints de anúncios e descontos respondem corretamente.
- O sistema mantém desempenho aceitável.
- O frontend consegue consumir os serviços externos.
- O uso do banco não apresenta consultas redundantes excessivas.
- A telemetria permite localizar falhas e gargalos.

---

## 4. Execução da Avaliação

### 4.1 Coleta e tratamento dos dados

Os dados foram consolidados a partir dos gráficos e evidências presentes no repositório, especialmente nas pastas de cenário quebrado e cenário correto. As figuras correspondentes podem ser usadas diretamente na apresentação e no relatório final.

#### A. Visão geral da arquitetura

No cenário base, o mapa de serviços mostra o frontend sofrendo impacto das falhas dos serviços downstream. No cenário corrigido, a arquitetura volta a operar de maneira estável.

- Cenário quebrado: [O Mapa do Problema](cenario-quebrado/O%20Mapa%20do%20Problema.png)
- Cenário correto: [O Mapa corrigido](cenario-correto/O%20Mapa%20corrigido.png)

#### B. M1 - Tolerância a falhas

No estado original, a falha do serviço de descontos e os problemas do frontend resultam em erros visíveis na experiência do usuário. Após a correção, a taxa de erro cai para zero.

- Cenário quebrado: [Erro grafico](cenario-quebrado/Erro%20grafico.png)
- Cenário correto: [Erro grafico](cenario-correto/Erro%20grafico.png)

**Valor esperado:** 0% de erros.

**Obtido no cenário base:** aproximadamente 5%.

**Obtido no cenário corrigido:** 0%.

#### C. M2 - Comportamento em relação ao tempo

O código original apresenta sleeps artificiais e comportamento bloqueante em serviços críticos. Isso gera latência superior a 2,5 segundos. Na versão corrigida, o tempo de resposta cai para milissegundos.

- Cenário quebrado: [Latencia](cenario-quebrado/Latencia.png)
- Cenário correto: [Latencia](cenario-correto/Latencia.png)

**Valor esperado:** abaixo de 100ms.

**Obtido no cenário base:** acima de 2.500ms.

**Obtido no cenário corrigido:** 6,79ms.

#### D. M3 - Utilização de recursos

Foi identificada a anomalia N+1 no serviço de descontos, causada por lazy loading no ORM. A versão corrigida adota eager loading e reduz drasticamente o número de consultas ao banco.

- Cenário quebrado: [Spam List](cenario-quebrado/Spam%20List.png)
- Cenário correto: [Spam List](cenario-correto/Spam%20List.png)

**Valor esperado:** menos de 50 queries.

**Obtido no cenário base:** acima de 208 queries para um carregamento.

**Obtido no cenário corrigido:** 42 queries.

### 4.2 Comparação com critérios

#### Resultado consolidado - cenário base

| Métrica | Valor observado | Pontuação | Peso | Contribuição |
|---|---:|---:|---:|---:|
| M1 - Erros HTTP | ~5% | 3 | 3 | 9 |
| M2 - Latência | > 2.500ms | 1 | 2 | 2 |
| M3 - Queries SQL | 208 | 1 | 2 | 2 |
| **Total** |  |  | **7** | **13** |

**Nota final do cenário base:** 13 / 7 = **1,85**

**Decisão:** descartar / refatoração crítica imediata.

#### Resultado consolidado - cenário corrigido

| Métrica | Valor observado | Pontuação | Peso | Contribuição |
|---|---:|---:|---:|---:|
| M1 - Erros HTTP | 0% | 5 | 3 | 15 |
| M2 - Latência | 6,79ms | 5 | 2 | 10 |
| M3 - Queries SQL | 42 | 4 | 2 | 8 |
| **Total** |  |  | **7** | **33** |

**Nota final do cenário corrigido:** 33 / 7 = **4,71**

**Decisão:** manter / aprovado com qualidade de excelência.

### 4.3 Análise crítica

#### Pontos fortes do software

- O repositório está bem estruturado para fins de demonstração de observabilidade.
- Há instrumentação em múltiplos níveis: frontend, backend, logs, APM, RUM e synthetics.
- A arquitetura permite comparar versões quebradas e corrigidas com clareza.
- A documentação e os artefatos visuais facilitam a identificação de gargalos e erros.

#### Pontos de melhoria

- O estado original contém falhas reais no código, como sleeps artificiais e N+1 query.
- Há erros de implementação que afetam endpoints e ações de criação de registros.
- O uso de tratamento genérico de exceções reduz a precisão do diagnóstico.
- A versão inicial não é adequada como base de produção sem correção.

#### Recomendação

Com base nos dados analisados, a versão inicial deve ser considerada inadequada para uso produtivo. A versão corrigida apresenta desempenho e confiabilidade significativamente melhores, sendo a recomendação final **manter a versão corrigida** e tratar a versão original como referência didática de problemas a serem detectados por observabilidade.

---

## Síntese Final para Apresentação

### Software avaliado

ecommerce-workshop / StoreDog.

### Métricas utilizadas

- Taxa de erros HTTP.
- Latência / tempo de resposta.
- Volume de consultas ao banco de dados.

### Processo de avaliação

1. Definição dos requisitos e do modelo de qualidade.
2. Escolha de métricas e critérios de pontuação.
3. Planejamento da coleta com Datadog APM e tráfego simulado.
4. Comparação entre cenário quebrado e cenário corrigido.
5. Cálculo das notas e análise crítica final.

### Principais resultados

- O cenário base recebeu nota 1,85 e foi classificado como crítico.
- O cenário corrigido recebeu nota 4,71 e foi classificado como excelente.
- O principal problema encontrado foi a combinação de falhas de confiabilidade, latência e N+1 query.

### Conclusão da avaliação

- Recomendação final: manter a versão corrigida.
- Motivo: a versão corrigida elimina os erros críticos e reduz drasticamente a latência e o custo de banco.

### Tempo sugerido

20 minutos por grupo.

## Apresentação

### O software avaliado

ecommerce-workshop / StoreDog.

### As métricas utilizadas

- Taxa de erros HTTP.
- Latência / tempo de resposta.
- Volume de consultas ao banco de dados.

### Processo de avaliação

- Definição do propósito da avaliação.
- Identificação do software e do contexto técnico.
- Seleção das características ISO/IEC 25010.
- Definição das métricas, pesos e critérios.
- Coleta de dados nos cenários quebrado e corrigido.
- Comparação dos resultados e conclusão crítica.

### Principais resultados

- O cenário base apresentou comportamento crítico, com nota 1,85.
- O cenário corrigido apresentou nota 4,71.
- Os gráficos mostram melhora em erros, latência e uso do banco.

### Conclusão da avaliação

- Recomendação final: manter a versão corrigida.
- Motivo: a versão corrigida elimina os erros críticos e reduz drasticamente a latência e o custo de banco.

## Entrega

### Relatório técnico

O relatório técnico deve conter:

- Descrição completa de todas as etapas.
- Instrumento de avaliação.
- Resultados obtidos.
- Gráficos e análise final.

### Arquivos de apoio

- Planilha / template ISO14598.
- Relatório técnico em formato editável.
- Evidências visuais organizadas nas pastas de resultados.
