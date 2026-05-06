# 2.3 Modelo de Qualidade ISO/IEC 25010

## Características selecionadas e pesos

Os pesos abaixo priorizam o funcionamento do fluxo de compra e a capacidade de detectar problemas em produção. Em um e-commerce, a função principal é vender com confiabilidade e desempenho aceitáveis; por isso, Adequação Funcional, Confiabilidade e Desempenho concentram a maior parte da ponderação.

| Característica | Peso | Justificativa |
|---|---:|---|
| Adequação Funcional | 35% | É o núcleo do negócio; compra, consulta e checkout precisam funcionar corretamente. |
| Confiabilidade | 20% | O sistema deve permanecer disponível e recuperar-se de falhas rapidamente. |
| Desempenho | 20% | Lentidão reduz conversão e aumenta abandono de carrinho. |
| Usabilidade | 15% | A interface deve ser clara para navegação e finalização de compra. |
| Compatibilidade | 5% | O sistema precisa funcionar em diferentes browsers e integrações. |
| Segurança | 5% | Dados pessoais e sessão devem ser protegidos. |
| Manutenibilidade | 3% | O código precisa ser sustentável para correções e evolução. |
| Transferibilidade | 2% | A aplicação deve ser portável entre ambientes e provedores. |
| **TOTAL** | **100%** |  |

## Adequação Funcional - 35%

**O que é:** avalia se o software executa corretamente as funções especificadas.

**Por que é importante para o e-commerce:** o fluxo de compra depende de rotas, integrações e respostas corretas dos serviços. Se o sistema falhar na listagem, exibição de banners, obtenção de descontos ou checkout, há impacto direto na operação e na receita.

**Subcaracterísticas a avaliar:**

- Completude: as funcionalidades essenciais estão presentes?
- Corretude: as saídas correspondem ao esperado?
- Apropriação: as funções implementadas são adequadas ao objetivo do negócio?

## Confiabilidade - 20%

**O que é:** mede a capacidade do sistema de funcionar sem falhas e manter disponibilidade.

**Por que é importante:** o workshop mostra que indisponibilidade e respostas 500 afetam imediatamente a experiência do usuário. Em produção, um e-commerce precisa responder de forma contínua, inclusive em picos de acesso.

**Subcaracterísticas a avaliar:**

- Maturidade: frequência de falhas e exceções.
- Disponibilidade: tempo em que os serviços permanecem acessíveis.
- Recuperação de falhas: capacidade de voltar ao estado estável após erro.

## Desempenho - 20%

**O que é:** avalia tempo de resposta, consumo de recursos e capacidade de processamento.

**Por que é importante:** o próprio repositório demonstra problemas de latência por sleeps artificiais e consultas ineficientes. Em um e-commerce, páginas lentas degradam conversão e pioram a experiência de navegação.

**Subcaracterísticas a avaliar:**

- Tempo de resposta.
- Throughput.
- Uso de recursos de CPU, memória e banco de dados.

## Usabilidade - 15%

**O que é:** mede a facilidade com que o usuário entende e opera a interface.

**Por que é importante:** a experiência de compra depende de navegação clara, leitura rápida de ofertas e execução simples de ações como salvar desconto ou avançar no checkout.

**Subcaracterísticas a avaliar:**

- Reconhecibilidade.
- Aprendizado.
- Operabilidade.
- Atratividade.

## Compatibilidade - 5%

**O que é:** avalia a capacidade de funcionar em diferentes plataformas e de integrar-se com outros sistemas.

**Por que é importante:** o e-commerce precisa operar em múltiplos navegadores e dialogar com serviços externos, como observabilidade, banco de dados e componentes de imagem ou conteúdo.

**Subcaracterísticas a avaliar:**

- Coexistência.
- Interoperabilidade.

## Segurança - 5%

**O que é:** avalia proteção contra acesso indevido, alteração indevida e perda de confidencialidade.

**Por que é importante:** mesmo em ambiente didático, o sistema trata navegação, contexto de usuário e integração com serviços externos. Um e-commerce real exigiria proteção rigorosa de dados pessoais e de sessão.

**Subcaracterísticas a avaliar:**

- Confidencialidade.
- Integridade.
- Autenticidade.
- Responsabilidade e rastreabilidade.

## Manutenibilidade - 3%

**O que é:** mede quão fácil é entender, alterar e testar o código.

**Por que é importante:** o repositório contém múltiplas versões e patches, o que exige organização mínima para permitir correções e comparação entre estados.

**Subcaracterísticas a avaliar:**

- Analisabilidade.
- Modificabilidade.
- Testabilidade.

## Transferibilidade - 2%

**O que é:** avalia a portabilidade para outros ambientes, servidores ou provedores.

**Por que é importante:** o projeto já inclui variantes para Docker, Kubernetes e diferentes nuvens, o que torna a portabilidade um critério útil, ainda que secundário em relação às características centrais do negócio.

**Subcaracterísticas a avaliar:**

- Adaptabilidade.
- Instalabilidade.
