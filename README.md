# Análise de Qualidade de Software — ecommerce-workshop (StoreDog)

Este repositório contém a análise técnica e os artefatos produzidos para a
avaliação de qualidade do projeto *ecommerce-workshop* (também referido como
StoreDog). O objetivo foi aplicar princípios da ISO/IEC 14598 e do modelo
ISO/IEC 25010, comparar um *cenário quebrado* com um *cenário corrigido* e
organizar evidências para apresentação.

Este README resume o que foi feito, onde está cada coisa e como usar os
arquivos para preparar a apresentação ou relatório final.

**Autor:** Grupo de avaliação
**Data:** ver metadados dos documentos em `04-resultados/resultado.md`

**Aviso:** O repositório original `ecommerce-workshop` na mesma workspace NÃO
foi modificado. Aqui armazenamos apenas a análise, reorganização de imagens
e relatórios gerados.

**Conteúdo principal deste repositório**

- **01-requisitos/**: documentos iniciais que descrevem o escopo, objetivos
	e a seleção de características ISO/IEC 25010.
	- [01-requisitos/analise-sistema.md](01-requisitos/analise-sistema.md)
	- [01-requisitos/objetivo-avaliacao.md](01-requisitos/objetivo-avaliacao.md)

- **02-metricas/**: (placeholder) local para a planilha e detalhes das métricas
	quando forem formalizadas em planilha.

- **03-plano/**: (placeholder) plano detalhado de execução da avaliação.

- **04-resultados/**: consolidação dos resultados, evidências e imagens usadas
	na análise.
	- Relatório consolidado: [04-resultados/resultado.md](04-resultados/resultado.md)
	- Evidências (imagens):
		- [04-resultados/cenario-correto](04-resultados/cenario-correto)
		- [04-resultados/cenario-quebrado](04-resultados/cenario-quebrado)

- **docs/**: documentos auxiliares como arquitetura, stack técnico e anexos.

- **slides/**: roteiro literal para os slides e dicas de apresentação.
	- [slides/Slides.md](slides/Slides.md)

- **templates/**: modelos (ex.: Relatório Técnico Modelo, planilha ISO14598).


**Resumo do que foi feito**

- Inspeção do código-fonte do `ecommerce-workshop` para identificar falhas
	intencionais (ex.: sleeps artificiais, N+1 query, typos, exceções genéricas).
- Definição das métricas principais (M1: taxa de erros HTTP; M2: latência; M3:
	volume de queries SQL), pesos e faixas de pontuação com base em ISO/IEC 25010.
- Coleta e consolidação de evidências visuais (prints e gráficos) nos cenários
	*quebrado* e *corrigido*.
- Cálculo das notas finais por média ponderada e elaboração do relatório
	consolidado em `04-resultados/resultado.md`.
- Criação do roteiro de apresentação em `slides/Slides.md` com conteúdo literal
	dos slides para uso direto em uma apresentação de ~15 minutos.


**Principais resultados (síntese rápida)**

- Cenário base (quebrado): nota final **1,85** — classificado como crítico.
	- Erros: ~5%
	- Latência média: > 2.500 ms
	- Queries por carregamento: ~208

- Cenário corrigido: nota final **4,71** — classificado como excelente.
	- Erros: 0%
	- Latência média: 6,79 ms
	- Queries por carregamento: 42

Detalhes e justificativas estão em [04-resultados/resultado.md](04-resultados/resultado.md).


**Como preparar a apresentação (passos rápidos)**

1. Abrir [slides/Slides.md](slides/Slides.md) e copiar o texto literal para o
	 editor de slides preferido (PowerPoint / Google Slides). O arquivo contém
	 sugestão de divisão entre 4 apresentadores e tempo por tópico.
2. Nas lâminas de resultados, inserir as imagens de
	 [04-resultados/cenario-quebrado](04-resultados/cenario-quebrado) e
	 [04-resultados/cenario-correto](04-resultados/cenario-correto).
3. Incluir breves notas do apresentador conforme a seção de fala de cada
	 integrante no final de `slides/Slides.md`.


**Observações para quem vai entregar o relatório**

- Se precisar de um relatório .docx formatado ou de uma planilha ISO14598
	preenchida, posso gerar um esqueleto a partir dos textos existentes.
- Os dados numéricos citados no relatório foram extraídos das figuras e
	evidências organizadas em `04-resultados`. Se desejar, posso anexar as
	capturas originais (PNG) em um arquivo ZIP para submissão.


**Tarefas pendentes (sugestão)**

- Preencher `02-metricas/` com a planilha detalhada e fórmulas.
- Gerar uma versão .pptx a partir de `slides/Slides.md` (automatizar).
- Validar a planilha ISO14598 e exportar o Relatório Técnico Modelo em .docx.


**Controle de versão**

Este diretório foi criado para agregar a análise; alteramos apenas arquivos
neste repositório de análise. O repositório `ecommerce-workshop` original
nesta workspace permaneceu inalterado.


---

Se quiser, eu já gero a versão `.pptx` com os slides prontos ou atualizo o
`README.md` com instruções de commit / push para preparar o artefato final.
