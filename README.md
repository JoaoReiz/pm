# Alocação escolar capacitada em Minas Gerais

Projeto de GCC118 — Programação Matemática. A etapa atual compara a alocação da demanda estimada entre escolas públicas existentes em Lavras e nas cinco maiores cidades mineiras por população: Belo Horizonte, Uberlândia, Contagem, Juiz de Fora e Montes Claros.

A análise usa todos os setores censitários de cada município, inclusive os de demanda zero. As três etapas são resolvidas separadamente. E1 atribui cada setor à escola mais próxima. E2 minimiza a distância ponderada e respeita capacidades hipotéticas. Todas as escolas elegíveis permanecem disponíveis. Não há seleção de unidades nem amostragem de setores.

## Notebooks principais

Os notebooks de AED permanecem como parte central do projeto:

- [AED Brasil](src/AED_Brasil.ipynb) faz o inventário nacional e por UF.
- [AED por UF](src/AED_UF.ipynb) caracteriza o conjunto de municípios de Minas Gerais.
- [AED Municipal](src/AED_Municipal.ipynb) explora a distribuição espacial dos setores e dados municipais.
- [AED Escolas](src/AED_Escolas.ipynb) analisa a oferta escolar, matrículas, faixas etárias e cenários de demanda e capacidade.

Para preparar e analisar os experimentos completos das seis cidades, use estes dois notebooks complementares, nesta ordem:

1. [Preparação das instâncias](src/Preparacao_Grandes_Municipios.ipynb) filtra escolas do INEP 2025, integra setores e grupos etários originais do IBGE 2022, audita coordenadas e cria matrizes completas.
2. [Formulação, testes com Gurobi e análise dos resultados](src/Experimentos_Grandes_Municipios.ipynb) implementa E1 e E2 em PuLP 2.9.0, resolve as instâncias com Gurobi 13.0.3 e consolida tabelas e gráficos. A retomada usa assinatura dos dados e parâmetros.

E2 usa `ceil(fator × matrículas)`, com fatores 1,00, 1,25 e 1,50. Matrículas indicam o porte observado, não vagas disponíveis ou capacidade física. A distância euclidiana entre centroides de setores e escolas é uma aproximação em linha reta.

## Dados locais

As bases públicas ficam em `data/` e não são incluídas no Git. A preparação espera:

- `BR_setores_CD2022.gpkg` do IBGE
- `Agregados_por_setores_demografia_BR.csv` do IBGE
- `Análise - Tabela da lista das escolas - Detalhado.csv` do Catálogo de Escolas do INEP
- `POP2026_20261029.xlsx` do IBGE para selecionar as cinco maiores cidades
- `Tabela_Escola_2025_V2.csv`, `Tabela_Matricula_2025_V2.csv` e `Tabela_Turma_2025_V2.csv` do Censo Escolar

O arquivo `predios_escolas_sit_pivot.xlsx` da SEE-MG foi usado como conferência complementar durante a curadoria. As coordenadas adicionais aceitas e as pendências estão documentadas em `data/geocodificacao_aprovada_grandes_municipios.csv` e `data/revisao_coordenadas_grandes_municipios.csv`.

Instale as dependências com `uv sync`. Para notebooks, use `uv run --with notebook jupyter notebook`. A licença WLS do Gurobi deve ser instalada localmente e nunca enviada ao GitHub. O solver precisa de acesso à internet para autenticar a licença acadêmica.

## Relatório

O relatório editável, o PDF e a versão Markdown ficam em `documentos/`. As figuras usadas pelo Markdown estão em `documentos/figuras/`. Os resultados completos, matrizes e atribuições ficam em `outputs/`, que é local e ignorado pelo Git devido ao tamanho. O relatório registra totais e indicadores consolidados.

## Arquivo histórico

`src/historico/` preserva localmente notebooks redundantes de formulações e experimentos anteriores. `documentos/versoes/` guarda rascunhos antigos e gráficos substituídos. Esses arquivos estão excluídos do Git para manter o repositório focado na análise atual.
