# Localização-alocação escolar — reconstrução em notebooks

Esta pasta contém **uma proposta alternativa**, preservando os notebooks históricos em `src/`. O trabalho é exploratório e não comprova viabilidade física, financeira ou fundiária de novas escolas.

**Ordem:** `00_problema_e_premissas` → `01_demanda_ibge` → `02_escolas_inep` → `03_instancias_candidatos` → `04_modelos_gurobi` → `05_resultados`.

A demanda é baseada em coortes demográficas do IBGE, com cenários explícitos de participação na rede pública; **não** é ajustada para coincidir com matrículas do INEP. Matrículas são uma medida observada de utilização, enquanto a capacidade é uma **proxy** obtida de turmas e cenários de lotação. Candidatos a escolas novas são locais abstratos de análise; não são lotes públicos validados.

### Fontes locais necessárias (não versionadas)
- `data/BR_setores_CD2022.gpkg`
- `data/Agregados_por_setores_demografia_BR.csv`
- diretório com `Tabela_Escola_2025_V2.csv`, `Tabela_Matricula_2025_V2.csv`, `Tabela_Turma_2025_V2.csv`
- `data/Análise - Tabela da lista das escolas - Detalhado.csv`
- correções auditadas em `data/` quando aplicável

Instalação `uv sync`; use `uv run --with notebook jupyter notebook`. Gurobi requer licença válida (não fornecida no Git). Todos os outputs vão para `outputs/localizacao_escolar/lavras`, ignorado pelo Git. **Não confundir ausência de execução com resultado experimental.**