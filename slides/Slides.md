# Roteiro de Apresentação - Seminário de Medição da Qualidade de Software

Este arquivo serve para montar a apresentação de aproximadamente 15 minutos do grupo. A ideia é usar somente o que é mais importante para a banca: contexto do software, critérios de qualidade, métricas escolhidas, resultados e conclusão.

## 1. Explicação geral da apresentação

### O que precisa ser explicado em 15 minutos

Para uma apresentação curta, o mais importante é mostrar que o grupo entendeu três coisas:

1. Qual software foi avaliado e por que ele foi escolhido.
2. Quais critérios de qualidade foram usados para avaliar o sistema.
3. Quais foram os resultados observados e qual é a conclusão final.

O foco não deve ser ler o relatório inteiro. O ideal é explicar apenas o que sustenta a análise: o sistema é um e-commerce com microserviços, o código tem problemas intencionais, o grupo definiu métricas claras e os resultados mostram diferença entre o cenário quebrado e o corrigido.

### O que vale mais tempo na fala

- Contexto do software e arquitetura.
- Métricas e pesos.
- Resultados com gráficos.
- Conclusão final.

### O que vale menos tempo

- Detalhes muito longos sobre norma.
- Listas extensas de funcionalidades.
- Explicações repetidas sobre o mesmo gráfico.

### Divisão sugerida para 4 pessoas

- Pessoa 1: introdução, objetivo e software avaliado.
- Pessoa 2: modelo de qualidade, métricas e pesos.
- Pessoa 3: resultados, gráficos e comparação dos cenários.
- Pessoa 4: análise crítica, conclusão e fechamento.

### Distribuição de tempo sugerida

- Abertura e contexto: 2 minutos.
- Requisitos e métricas: 4 minutos.
- Resultados e gráficos: 5 minutos.
- Conclusão e fechamento: 4 minutos.

---

## 2. Partes que devem constar no slide

### Slide 1 - Capa

Deve conter:

- Nome do trabalho.
- Nome do software avaliado.
- Nome dos integrantes.
- Disciplina e data.

### Slide 2 - Contexto do software

Deve conter:

- O que é o ecommerce-workshop.
- Qual é o objetivo do sistema.
- Qual é a stack geral.
- Por que ele foi escolhido.

### Slide 3 - Objetivo da avaliação

Deve conter:

- Por que o software está sendo avaliado.
- Qual é o contexto da avaliação.
- O que o grupo quer descobrir com a análise.

### Slide 4 - Modelo de qualidade ISO/IEC 25010

Deve conter:

- As características escolhidas.
- O motivo de cada escolha.
- O peso de cada característica principal.

### Slide 5 - Métricas e critérios

Deve conter:

- As métricas definidas.
- Os níveis de pontuação.
- Os critérios de julgamento.

### Slide 6 - Plano e coleta

Deve conter:

- Como os dados foram coletados.
- Quais ferramentas foram usadas.
- Quais cenários foram comparados.

### Slide 7 - Resultados

Deve conter:

- Valores observados.
- Gráficos dos cenários quebrado e corrigido.
- Comparação entre esperado e obtido.

### Slide 8 - Conclusão

Deve conter:

- Pontos fortes.
- Pontos de melhoria.
- Recomendação final.

### Slide 9 - Encerramento

Deve conter:

- Mensagem final do grupo.
- Frase de fechamento.
- Espaço para perguntas.

---

## 3. Conteúdo literal dos slides

### Slide 1 - Capa

**Seminário de Medição da Qualidade de Software**

**Avaliação do ecommerce-workshop / StoreDog**

Grupo: [nomes dos integrantes]

Disciplina: [nome da disciplina]

Data: [data da apresentação]

### Slide 2 - Contexto do software

**O que é o ecommerce-workshop?**

O ecommerce-workshop é um repositório de laboratório criado para demonstrar observabilidade em um cenário de e-commerce. O sistema combina um storefront em Ruby on Rails (Spree) com microsserviços em Python (Flask) para descontos e anúncios, conectados a bancos PostgreSQL e SQLite.

**Por que esse software foi escolhido?**

Porque ele permite avaliar um sistema real, com código-fonte acessível, integração entre serviços e problemas intencionais de desempenho e confiabilidade, o que é ideal para aplicar ISO/IEC 14598 e ISO/IEC 25010.

### Slide 3 - Objetivo da avaliação

**Objetivo da avaliação**

A avaliação foi feita para verificar se o sistema apresenta qualidade suficiente para um cenário de e-commerce, analisando confiabilidade, desempenho e qualidade geral de operação.

**Pergunta principal**

O sistema está funcional, estável e rápido o suficiente para ser considerado adequado no cenário corrigido?

### Slide 4 - Modelo de qualidade ISO/IEC 25010

**Características avaliadas**

- Confiabilidade (Tolerância a Falhas) - 60%
- Eficiência de Desempenho (Tempo e Recursos) - 40%

**Justificativa**

Um e-commerce não pode cair se um serviço de anúncio ou desconto falhar, por isso a confiabilidade recebe maior peso. A lentidão afeta diretamente as vendas e o custo de infraestrutura, então a eficiência de desempenho também recebe peso alto. Como o foco do trabalho foi a comparação entre cenário quebrado e corrigido, essas duas dimensões resumem melhor o problema avaliado.

### Slide 5 - Métricas e critérios

**Métricas definidas**

- M1: Taxa de erros HTTP.
- M2: Tempo de resposta / latência.
- M3: Volume de consultas ao banco de dados.

**Critério de julgamento**

As notas foram calculadas por média ponderada. O cenário base recebeu nota 1,85, sendo classificado como crítico. O cenário corrigido recebeu nota 4,71, sendo classificado como excelente.

### Slide 6 - Plano e coleta

**Como os dados foram coletados**

Os dados foram observados por meio de Datadog APM, Service Map, tráfego simulado com GoReplay e comparação entre o cenário quebrado e o cenário corrigido.

**O que foi comparado**

- Erros HTTP.
- Latência.
- Número de queries SQL.

### Slide 7 - Resultados

**Cenário base**

- Erros: cerca de 5%.
- Latência: acima de 2.500ms.
- Queries: acima de 208.
- Nota final: 1,85.

**Cenário corrigido**

- Erros: 0%.
- Latência: 6,79ms.
- Queries: 42.
- Nota final: 4,71.

**Interpretação**

O sistema original apresenta falhas críticas de confiabilidade e desempenho. A versão corrigida melhora fortemente os indicadores e atende melhor aos critérios definidos.

### Slide 8 - Conclusão

**Pontos fortes**

- O projeto é bem instrumentado para observabilidade.
- Há documentação e evidências visuais que facilitam a análise.
- A comparação entre versões ajuda a entender os problemas com clareza.

**Pontos de melhoria**

- O estado inicial tinha sleeps artificiais, erro de implementação e N+1 query.
- O sistema original não é adequado como base de produção.

**Recomendação final**

Manter a versão corrigida do sistema e tratar a versão original como cenário didático de problemas de qualidade.

### Slide 9 - Encerramento

**Obrigado!**

Fim da apresentação.

Perguntas?

---

## 4. Sugestão de fala por integrante

### Integrante 1

Abrir a apresentação, explicar o que é o sistema e qual foi o objetivo da avaliação.

### Integrante 2

Explicar a ISO/IEC 25010, os critérios escolhidos e as métricas.

### Integrante 3

Apresentar os gráficos, os números e a comparação entre os dois cenários.

### Integrante 4

Fechar com a análise crítica, os pontos fortes, os pontos de melhoria e a recomendação final.

---

## 5. Dica prática para a apresentação

Se o tempo apertar, priorize os slides 2, 4, 5, 7 e 8. Eles carregam praticamente toda a nota da apresentação porque mostram contexto, método, resultado e conclusão.
