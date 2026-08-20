# Databricks PrimeirosComandos ManipulacaoArquivos.

- <p>Vamos comecar a organizar o nosso Notebooks.</p>
 Um notebook do Databricks é um documento interativo na web onde você escreve, executa e compartilha código de análise de dados e inteligência artificial em tempo real.

### 01. Organizacao de Databricks Notebooks

No Databricks, a Organização de Notebooks refere-se ao conjunto de boas práticas, estruturas de diretórios e padrões de projeto usados para estruturar o código (Python, SQL, Scala, R) no Workspace do Databricks de forma sustentável, colaborativa e pronta para produção.

Em ambientes corporativos, organizar os notebooks deixa de ser apenas uma questão de preferência pessoal e passa a ser essencial para a engenharia de dados, qualidade de dados e esteiras de CI/CD.

1. Estrutura de Diretórios e Escopo de Uso
A organização começa pela divisão do espaço de trabalho do workspace em três grandes categorias:

Users/ (Espaço Pessoal / Sandbox): Diretórios individuais para desenvolvimento inicial, testes locais e exploração de dados sem risco de impactar outros usuários.

Shared/ (Projetos Compartilhados): Utilizado para bibliotecas comuns, rotinas reutilizáveis e código colaborativo entre equipes.

Production/ ou Projects/ (Ambiente Oficial): Espaço estruturado conectado ao controle de versão (Git) onde residem as pipelines oficiais executadas em produção via Workflows/Jobs.

2. Estrutura por Arquitetura de Dados (Medallion Architecture)
Em projetos de Engenharia de Dados, a organização dos notebooks costuma acompanhar as camadas da arquitetura Medalhão:

```text
📁 ETL_Pipeline/
├── 📄 00_setup_config.py      # Variáveis globais, conexões e funções auxiliares
├── 📄 01_ingestion_bronze.py  # Leitura das fontes e carga no catálogo em estado bruto
├── 📄 02_transform_silver.py  # Limpeza, deduplicação, validações e qualidade de dados
└── 📄 03_aggregate_gold.py    # Regras de negócio, agregados e tabelas prontas para consumo
````


Modularização via %run ou Módulos Python (.py):

%run ./notebook_auxiliar: Executa um notebook dentro de outro, importando suas variáveis e funções (muito usado para scripts de configuração).

Arquivos .py nativos: O Databricks permite importar arquivos .py comuns armazenados na mesma estrutura de pastas como módulos Python convencionais (import my_module).

Parametrização com Widgets: Uso de dbutils.widgets para capturar parâmetros de entrada dinamicamente (ex: data de execução, ambiente dev/prod), evitando que valores fixos (hardcoded) fiquem espalhados no código.

Documentação Embutida (Markdown): Uso do comando %md para documentar o objetivo do script, entradas, saídas e regras de negócio aplicadas nas células.

3. Principais Recursos de Organização no Databricks
Databricks Repos / Git Integration: Permite sincronizar pastas de notebooks diretamente com repositórios remotos (GitHub, Azure DevOps, GitLab, Bitbucket). Isso viabiliza versionamento por branches, code reviews e controle de implantação via CI/CD.

| Aspecto | Prática Inadequada | Boa Prática de Organização |
| :--- | :--- | :--- |
| **Escopo do Script** | Notebooks monolíticos com milhares de linhas executando Ingestão e Carga juntas. | Notebooks pequenos e focados em uma única responsabilidade no fluxo. |
| **Versionamento** | Código alterado manualmente no workspace sem registro de histórico. | Notebooks vinculados a Reposiórios Git (GitHub, Azure DevOps) com controle por branch. |
| **Parametrização** | Mudar variáveis manualmente no código a cada execução. | Uso de **Widgets** e integração com parâmetros de **Databricks Jobs**. |
| **Reutilização** | Código duplicado e copiado em múltiplos notebooks. | Funções utilitárias consolidadas em módulos Python ou notebooks utilitários. |

Acessar DataBricks / Workspace

<img width="1359" height="503" alt="image" src="https://github.com/user-attachments/assets/80ad51fa-f259-45d3-b9a5-7325094f921d" />

Dentro do Worspace / Users / Selecionar usuario

<img width="1363" height="446" alt="image" src="https://github.com/user-attachments/assets/b814d33f-65ae-4844-9e3c-605496aaf2bc" />

Criar uma nova pasta

<img width="828" height="418" alt="image" src="https://github.com/user-attachments/assets/adc3ebe3-5fb1-44b4-90aa-62c4397948db" />

<img width="946" height="438" alt="image" src="https://github.com/user-attachments/assets/f966486d-0d74-4ab0-a0ec-23142066c01e" />

Criar uma Subpasta chamada Links importantes

<img width="720" height="414" alt="image" src="https://github.com/user-attachments/assets/06b27a5d-1644-419f-af76-6852b3630dcf" />

Criar uma nova subpasta chamada Comandos Basicos

<img width="658" height="240" alt="image" src="https://github.com/user-attachments/assets/944c7898-7732-4367-9a3a-69d2a5c34258" />

Dentro de Comandos basicos vamos criar o nosso primeiro notebook

<img width="871" height="503" alt="image" src="https://github.com/user-attachments/assets/96095b37-9408-4140-9ccc-b331f9822fed" />

Renomear o notebook para testes

<img width="753" height="364" alt="image" src="https://github.com/user-attachments/assets/3bfffc5f-73a9-4c09-a0bf-a46eb213deda" />

Vamos Remover esse Notebook de testes

<img width="1108" height="581" alt="image" src="https://github.com/user-attachments/assets/6f481ac5-feb5-4c12-af0f-a3bf43fbadf4" />

### 02. Hierarquias Databricks - Catalog, Schema, Tabelas e Volumens

O Unity Catalog é a solução de governança unificada e centralizada de dados e inteligência artificial da Databricks.

Ele funciona como uma camada única de controle aplicada a todas as áreas de trabalho (workspaces) do Databricks na sua nuvem (AWS, Azure ou GCP).

Dentro do Databricks Unity Catalog, esses elementos formam a estrutura hierárquica para governança, organização e controle de acesso aos dados:

Catalog (Catálogo): O nível mais alto de organização (container primário). Agrupa esquemas e serve como a principal fronteira para definir permissões de governança e isolamento de ambientes (ex: prod, dev).

Schema (Esquema / Database): O segundo nível da hierarquia, contido dentro de um Catalog. Funciona como uma pasta/diretório para organizar logicamente ativos relacionados (tabelas, visões e volumes).

Tabelas (Tables): Objetos de dados estruturados e tabulares (com colunas e linhas) gerenciados pelo Unity Catalog (geralmente no formato Delta Lake), usados para consultas SQL, análises e machine learning.

Volumes (Volumes): Objetos que representam armazenamento de dados não estruturados ou semi-estruturados (como imagens, arquivos PDF, JSONs brutos, CSVs ou logs). Permitem acessar e governar arquivos diretamente no armazenamento em nuvem sem precisar convertê-los em tabelas.

| Nível | Função Principal | Tipo de Dado Contido |
| :--- | :--- | :--- |
| **Catalog** | Governança e isolamento de alto nível | Schemas |
| **Schema** | Agrupamento lógico por contexto/projeto | Tabelas, Views, Volumes |
| **Tabelas** | Dados estruturados para consulta SQL/Analytics | Linhas e Colunas (Delta) |
| **Volumes** | Armazenamento governado de arquivos brutos | Arquivos não estruturados |

<img width="546" height="411" alt="image" src="https://github.com/user-attachments/assets/9464e199-d287-4bb8-938f-139820b77202" />

<img width="1343" height="419" alt="image" src="https://github.com/user-attachments/assets/e3406691-85d2-4186-bb05-2e210ce09267" />

### Criando Hierarquia catalog na prática

Aqui eu posso escolher a linguagem que eu quero escrever

<img width="1259" height="521" alt="image" src="https://github.com/user-attachments/assets/925eed28-03cd-434c-bf99-2d813997a3c1" />

Para criar novas celulas de código

<img width="1108" height="470" alt="image" src="https://github.com/user-attachments/assets/079b4e74-ea59-4cb1-a929-97464e45b5e9" />

- Criando um catalogo utilizando o exemplo da Microsoft

Create catalogs: https://learn.microsoft.com/en-us/azure/databricks/catalogs/create-catalog

<img width="715" height="149" alt="image" src="https://github.com/user-attachments/assets/8c0e4ae0-768b-4e64-be07-e4e556ebf41b" />

<img width="752" height="457" alt="image" src="https://github.com/user-attachments/assets/c5c3dc1d-2b43-4de9-9055-0f6be746d0de" />




















