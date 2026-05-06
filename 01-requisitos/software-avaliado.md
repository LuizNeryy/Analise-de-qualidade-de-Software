# 2.2 Software Avaliado

## Nome do Software

ecommerce-workshop

## Descrição Geral

O ecommerce-workshop é um repositório de demonstração voltado à observabilidade de um sistema de e-commerce. O cenário parte de uma loja baseada em Spree/Rails e adiciona serviços auxiliares em Python/Flask para anúncios e descontos, além de um frontend em React para experimentação com RUM. O projeto inclui ainda infraestrutura de implantação e observabilidade para múltiplos ambientes.

O código não representa uma aplicação comercial finalizada. Ele foi estruturado para ensinar identificação de falhas, análise de latência, rastreamento distribuído, comparação de versões de deploy e inspeção de comportamento do usuário no frontend.

## Objetivo do Software

O ecommerce-workshop é um ambiente didático de e-commerce, desenvolvido para demonstrar práticas de observabilidade, diagnóstico de incidentes e análise de desempenho em uma arquitetura distribuída. Permite que usuários naveguem por produtos, consultem descontos, visualizem anúncios e acompanhem o comportamento do sistema por meio de Datadog.

## Principais Funcionalidades

- Navegação de produtos no storefront principal.
- Fluxo de checkout e atualização de pedido.
- Consulta de descontos por serviço dedicado.
- Exibição de anúncios e banners em diferentes versões.
- Listagem, ordenação e salvamento local de descontos no frontend React.
- Instrumentação de APM, logs, RUM e synthetics.
- Replay de tráfego para geração de carga e investigação.
- Comparação entre versões de serviços para análise de deploy.

## Público-Alvo

O software é direcionado a estudantes, instrutores e equipes que desejam praticar observabilidade, análise de incidentes e instrumentação em um cenário de e-commerce com microserviços.

## Contexto Técnico

- Stack: Ruby on Rails/Spree, Python Flask, React 17, TypeScript, Vite, PostgreSQL, SQLite e Datadog.
- Tipo: aplicação web distribuída.
- Número de usuários esperados: baixo a médio, suficiente para demonstração acadêmica e reprodução de incidentes.
- Criticidade: média no cenário de estudo, pois a perda de disponibilidade ou lentidão afeta diretamente a experiência de compra.
