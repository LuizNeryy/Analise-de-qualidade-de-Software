# Arquitetura do Sistema

O ecommerce-workshop adota uma arquitetura híbrida. O storefront principal é um aplicativo Rails/Spree que expõe o catálogo, a navegação e o checkout. Ao lado dele, há microserviços em Flask para descontos e anúncios, conectados a bancos PostgreSQL e SQLite.

## Camadas principais

- Apresentação: storefront Rails.
- Serviços: descontos e anúncios em Flask.
- Dados: PostgreSQL para os microserviços e SQLite no estado inicial do storefront.
- Observabilidade: Datadog Agent, APM, RUM, logs e synthetics.
- Implantação: Docker Compose, Kubernetes, Terraform e variações por provedor.

## Observações arquiteturais

- O repositório contém versões quebradas e corrigidas dos mesmos serviços.
- Há suporte a replay de tráfego para gerar telemetria sem intervenção manual.
- O projeto é orientado à demonstração de incidentes, não a uma arquitetura de produção finalizada.
