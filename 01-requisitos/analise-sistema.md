# 1. Análise do ecommerce-workshop

## 1.1 Resumo Executivo

O ecommerce-workshop é um repositório de laboratório voltado ao ensino de observabilidade em um cenário de e-commerce. O projeto simula uma loja virtual baseada em Spree/Rails e complementada por microserviços em Flask, com instrumentação Datadog para APM, logs, RUM, sintéticos e comparação de deploys. O objetivo não é fornecer um sistema comercial completo, mas reproduzir problemas reais de operação, latência, erros e integração entre serviços.

O código mostra uma arquitetura deliberadamente heterogênea. O storefront principal é um aplicativo Rails montado sobre Spree, com integrações para os serviços de descontos e anúncios. Há ainda uma aplicação auxiliar em React/Vite para listar descontos e banners, além de infraestrutura de suporte para Docker Compose, Kubernetes, Terraform, nginx, GoReplay e Datadog Agent.

### Principais componentes identificados

- Storefront principal em Rails/Spree.
- Serviço de descontos em Flask.
- Serviço de anúncios em Flask.
- Frontend auxiliar em React/Vite para descontos.
- Infraestrutura de observabilidade Datadog.
- Artefatos de implantação para Docker, Kubernetes, AWS, Azure, GCP, OpenShift e Terraform.

## 1.2 Stack Tecnológico Detalhado

### Frontend

- Aplicação principal de loja: Ruby on Rails com Spree.
- Aplicação auxiliar de descontos: React 17, TypeScript, Vite e Tailwind CSS.

### Backend

- Ruby on Rails para o storefront.
- Python com Flask para os microserviços de descontos e anúncios.

### Banco de dados

- PostgreSQL para os microserviços Flask.
- SQLite no estado inicial do storefront Rails, com schema e bases locais versionadas no repositório.

### Observabilidade

- Datadog APM.
- Datadog Browser RUM.
- Logs com correlação por serviço.
- Profiler em Python nos microserviços.
- Synthetics via arquivo de configuração do projeto.
- Service Catalog por definições YAML.

### Infraestrutura

- Docker e Docker Compose.
- Kubernetes.
- Nginx.
- Terraform para múltiplos provedores.
- Suporte a AWS, Azure, GCP e OpenShift.

### Outras ferramentas

- GoReplay para reprodução de tráfego.
- Datadog Agent.
- PostgreSQL 11-alpine.
- Scripts de build e publicação de imagens.

## 1.3 Estrutura do Projeto

### Pastas principais

- `ads-service`: microserviço de anúncios com sleeps artificiais.
- `ads-service-fixed`: versão corrigida do serviço de anúncios.
- `ads-service-errors`: variante com erros HTTP 500 no endpoint de anúncios.
- `ads-service-versions`: versões v1, v2 e v2.1 para comparação de deploy.
- `discounts-service`: microserviço de descontos com N+1 query e sleeps.
- `discounts-service-fixed`: versão corrigida do serviço de descontos.
- `discounts-react-app`: frontend auxiliar de descontos com RUM.
- `store-frontend`: storefront principal em Spree/Rails, com variações de build.
- `deploy`: manifests, docker-compose, Helm, Terraform e demais destinos.
- `service-definitions`: definições de catálogo de serviços do Datadog.
- `traffic-replay`: replay de tráfego com GoReplay.
- `attackbox`: scripts e chaves para cenário de ataque didático.
- `nginx`: configuração do proxy reverso.
- `images`: imagens e capturas usadas na documentação do workshop.

### Arquitetura geral

O sistema segue uma arquitetura de microserviços com um frontend monolítico principal e serviços auxiliares. O storefront consome o serviço de descontos e o serviço de anúncios. O frontend auxiliar em React também consome esses serviços para exibir descontos e banners. A observabilidade é centralizada no Datadog Agent e nos SDKs específicos de cada aplicação.

### Fluxo de integração

1. O usuário navega pelo storefront principal ou pelo frontend auxiliar.
2. O frontend chama os serviços Flask para obter descontos e anúncios.
3. O Datadog coleta traces, logs, métricas e eventos de RUM.
4. O tráfego de exemplo pode ser reproduzido pelo container `traffic-replay`.
5. As falhas e latências são analisadas no Datadog para orientar a correção.

## 1.4 Funcionalidades Principais

### Funcionalidades observadas no código

- Listagem de produtos no storefront Spree.
- Exibição de detalhe de produto.
- Fluxo de checkout e atualização de pedido.
- Renderização de banners de anúncios no storefront.
- Consulta de descontos a partir do microserviço Flask.
- Listagem de descontos no frontend React.
- Ordenação de descontos por nome, código e valor.
- Salvamento local de descontos favoritos no navegador.
- Alternância entre visualização de todos os descontos e apenas os salvos.
- Troca manual de anúncios no frontend React.
- Download e exibição de banners de anúncio por imagem.
- Replay de tráfego para simular usuários e gerar telemetria.

### Observação funcional

O repositório contém várias versões do mesmo domínio funcional. Algumas versões existem para demonstrar erros, latência ou correções, e não representam uma linha estável de produto.

## 1.5 Observabilidade Implementada

### Ferramentas disponíveis

- Datadog Agent em contêiner dedicado.
- APM nos serviços Python e Rails.
- RUM no frontend React.
- Logs estruturados e rotulados por serviço.
- Profiler Python habilitado em parte dos serviços.
- Service Catalog via `service-definitions/*.yaml`.
- Synthetics via `storedog.synthetics.json`.
- Replay de tráfego com GoReplay.

### Métricas e sinais coletados

- Latência dos serviços.
- Taxa de erro HTTP.
- Traces distribuídos entre frontend e microserviços.
- Ações de usuário no frontend React.
- Eventos de navegação e sessão via RUM.
- Logs de aplicação e do banco PostgreSQL.

### Como acessar

- Painel Datadog para APM, Logs, RUM e Service Map.
- Endpoints de serviço como `/ads`, `/discount` e rotas do Spree.
- Interface do frontend React para gerar ações instrumentadas.
- Container `traffic-replay` para aumentar o volume de requisições.

### Health checks e status

O projeto não expõe um conjunto uniforme de endpoints de health check. Em vez disso, o próprio workshop usa endpoints funcionais, páginas principais e observabilidade no Datadog para inferir saúde, disponibilidade e degradação.

## 1.6 Limitações e Problemas Encontrados

### Problemas intencionais no workshop

- O serviço de anúncios original possui sleeps artificiais de 2,5 segundos.
- O serviço de descontos original possui N+1 query e sleeps artificiais.
- Há variantes quebradas do serviço de anúncios com respostas 500.
- Existem versões de frontend e backend feitas especificamente para demonstrar correção de incidente.

### Bugs e fragilidades observadas

- `discounts-service/discounts.py` usa `random.randomint`, que não existe em Python e quebra o fluxo de criação de desconto.
- `ads-service/ads.py` referencia `discounts_count` ao criar anúncio, mas a variável correta é `advertisements_count`, o que provoca falha em POST.
- O serviço de anúncios e o serviço de descontos usam `except:` genérico, o que reduz a precisão do diagnóstico de erro.
- O frontend React ordena a lista diretamente com `sort()`, mutando o estado original antes de atualizar a interface.
- O frontend React cria `URL.createObjectURL()` para banners sem revogação explícita, o que pode gerar vazamento de memória em sessões longas.
- O storefront Rails expõe uma action `add` vazia em `DiscountsController`, indicando funcionalidade incompleta.
- A aplicação inclui chaves e scripts de ataque em `attackbox`, reforçando que o repositório foi desenhado para cenários de segurança e observabilidade, não para uso produtivo.
