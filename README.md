# Databricks PrimeirosComandos ManipulacaoArquivos.

- <p>Vamos comecar a organizar o nosso Notebooks.</p>
 Um notebook do Databricks é um documento interativo na web onde você escreve, executa e compartilha código de análise de dados e inteligência artificial em tempo real.

### 01. Organizacao de Databricks Notebooks

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














