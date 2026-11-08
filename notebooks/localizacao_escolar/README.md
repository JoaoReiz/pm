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
## Escopo final: metodologia nacional (Lavras é piloto)

O objetivo do projeto é aplicar a mesma metodologia, com dados públicos, a municípios em qualquer UF do Brasil. A instância de Lavras testa a abordagem e **não determina os parâmetros nacionais**. Os notebooks 01–05 atuais ainda implementam somente a fase-piloto e **não são um pipeline nacional pronto**.

### Metodologia nacional planejada
- **Demanda**: coortes demográficas do Censo IBGE 2022 por setor, correção de participação escolar/rede pública como cenários explícitos; preservar faltantes/sigilo e documentar recortes geográficos e anos diferentes.
- **Oferta**: escolas ativas e etapas oferecidas no INEP, capacidade potencial por salas de aula físicas auditadas; não inferir capacidade operacional das matrículas ou turmas observadas. Normalizar campos conforme o dicionário de cada ano.
- **Lotação**: cenários comparáveis de lotação por etapa, **não** tratados como limite legal nacional; quando houver norma municipal/estadual verificada, adicioná-la como parâmetro regional opcional.
- **Espaços compartilhados**: variáveis inteiras de salas atribuídas a cada etapa e turno, com restrição de uso simultâneo para evitar contar as mesmas salas em várias etapas; regimes integrais exigem reserva por mais de um turno.
- **Candidatos**: pontos de setores com demanda potencial funcionam apenas como aproximações espaciais. Não são terrenos disponíveis.
- **Nova escola**: cenário de porte ligado a projetos FNDE, quando aplicáveis à etapa e ao regime, sem extrapolar capacidades automaticamente.
- **Escalabilidade**: processar IBGE/INEP por UF e instanciar problemas por município (ou grupos limítrofes onde houver fluxos intermunicipais), com arcos de atribuição esparsos; jamais criar matriz densa setores × escolas para o Brasil todo.
- **Qualidade**: auditorias por município/etapa (ausência de salas, geocodificação, supressão, escola compartilhada, fronteiras urbanas/rurais). Relatar explicitamente cobertura dos dados antes de qualquer inferência.
- **Comparações**: número de novas instalações, não atendimento, deslocamento, status, tempo e análise de sensibilidade à demanda, lotação e turnos.

Fontes oficiais: [Agregados censitários 2022](https://www.ibge.gov.br/estatisticas/sociais/populacao/22827-censo-demografico-2022.html?edicao=41852&t=downloads), [Malha 2022](https://www.ibge.gov.br/geociencias/organizacao-do-territorio/malhas-territoriais/26565-malhas-de-setores-censitarios-divisoes-intramunicipais.html), [Censo Escolar INEP](https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/censo-escolar), [Projetos FNDE](https://www.gov.br/fnde/pt-br/acesso-a-informacao/acoes-e-programas/programas/par/infraestrutura-fisica-escolar/escola-9-salas-dois-pavimentos).

### Limitação fundamental

`QT_SALAS_UTILIZADAS`/campos equivalentes são informações sobre salas **utilizadas**, não necessariamente todas as salas disponíveis, adequadas ou desocupadas. Sem dimensões de cada sala, horários e pessoal, o modelo produz **capacidade potencial em cenários**, não capacidade autorizada. A hipótese de lotação deve ser distinguida de norma legal e de parâmetro pedagógico da rede.
