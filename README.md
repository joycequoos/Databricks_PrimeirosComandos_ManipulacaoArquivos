[← Voltar para Databricks com SQL, Python e PySpark](https://github.com/joycequoos/Databricks-com-linguagem-SQL-Python-PySpark-Para-An-lise-de-Dados-Cloud/blob/main/README.md)

# Databricks — Primeiros Comandos e Manipulação de Arquivos

Organização de notebooks e hierarquia de dados no Databricks: como estruturar o Workspace do jeito certo e como funciona o Unity Catalog (Catalog → Schema → Tabelas → Volumes).

## Sumário

- [Parte 1 — Organização de Notebooks](#parte-1--organização-de-notebooks)
  - [Onde cada tipo de código vive](#onde-cada-tipo-de-código-vive)
  - [Estrutura por Arquitetura Medalhão](#estrutura-por-arquitetura-medalhão)
  - [Boas práticas x Práticas inadequadas](#boas-práticas-x-práticas-inadequadas)
  - [Passo a passo: criando a estrutura de pastas](#passo-a-passo-criando-a-estrutura-de-pastas)
- [Parte 2 — Hierarquia do Unity Catalog](#parte-2--hierarquia-do-unity-catalog)
  - [A hierarquia em um diagrama](#a-hierarquia-em-um-diagrama)
  - [O que é cada nível](#o-que-é-cada-nível)
  - [Passo a passo: criando a hierarquia na prática](#passo-a-passo-criando-a-hierarquia-na-prática)
- [Resumo geral](#resumo-geral)

---

## Parte 1 — Organização de Notebooks

> **Resumo:** um notebook Databricks é um documento interativo onde se escreve, executa e compartilha código de análise de dados. Organizar bem esses notebooks deixa de ser preferência pessoal e vira requisito para engenharia de dados, qualidade e esteiras de CI/CD em ambiente corporativo.

### Onde cada tipo de código vive

O Workspace do Databricks se divide em três grandes áreas, cada uma com um propósito diferente:

```mermaid
graph TD
    W[Workspace Databricks]
    W --> U["👤 Users/<br/>Espaço pessoal · Sandbox<br/>testes e exploração sem risco"]
    W --> S["🤝 Shared/<br/>Projetos compartilhados<br/>bibliotecas e rotinas reutilizáveis"]
    W --> P["🚀 Production/ ou Projects/<br/>Ambiente oficial<br/>conectado ao Git · roda via Jobs"]
```

### Estrutura por Arquitetura Medalhão

Em projetos de Engenharia de Dados, os notebooks costumam seguir as camadas da arquitetura Medalhão — cada script com uma responsabilidade só:

```mermaid
graph LR
    A["00_setup_config.py<br/>variáveis, conexões, funções auxiliares"] --> B["01_ingestion_bronze.py<br/>leitura das fontes · carga bruta"]
    B --> C["02_transform_silver.py<br/>limpeza, deduplicação, qualidade"]
    C --> D["03_aggregate_gold.py<br/>regras de negócio · pronto para consumo"]
```

**Formas de modularizar o código:**

| Recurso | Para que serve |
|---|---|
| `%run ./notebook_auxiliar` | Executa um notebook dentro de outro, importando variáveis e funções |
| Arquivos `.py` nativos | Importados como módulos Python comuns (`import my_module`) |
| `dbutils.widgets` | Captura parâmetros de entrada dinamicamente (data, ambiente dev/prod) — evita valores fixos no código |
| `%md` | Documenta objetivo, entradas, saídas e regras de negócio direto nas células |

**Governança e versionamento:** o **Databricks Repos** sincroniza pastas de notebooks com repositórios remotos (GitHub, Azure DevOps, GitLab, Bitbucket), viabilizando versionamento por branch, code review e CI/CD.

### Boas práticas x Práticas inadequadas

| Aspecto | ❌ Prática inadequada | ✅ Boa prática |
|---|---|---|
| **Escopo do script** | Notebook monolítico com milhares de linhas fazendo ingestão e carga juntas | Notebooks pequenos, uma responsabilidade cada |
| **Versionamento** | Código alterado direto no workspace, sem histórico | Notebooks vinculados a repositório Git, com controle por branch |
| **Parametrização** | Variáveis trocadas manualmente a cada execução | Uso de Widgets + parâmetros de Databricks Jobs |
| **Reutilização** | Código duplicado em vários notebooks | Funções utilitárias em módulos/notebooks utilitários |

### Passo a passo: criando a estrutura de pastas

**1. Acessar o Databricks / Workspace**

<img width="1359" height="503" alt="Acessar Databricks Workspace" src="https://github.com/user-attachments/assets/80ad51fa-f259-45d3-b9a5-7325094f921d" />

**2. Dentro do Workspace, ir em Users e selecionar o usuário**

<img width="1363" height="446" alt="Selecionar usuário" src="https://github.com/user-attachments/assets/b814d33f-65ae-4844-9e3c-605496aaf2bc" />

**3. Criar uma nova pasta**

<img width="828" height="418" alt="Criar nova pasta" src="https://github.com/user-attachments/assets/adc3ebe3-5fb1-44b4-90aa-62c4397948db" />
<img width="946" height="438" alt="Nova pasta criada" src="https://github.com/user-attachments/assets/f966486d-0d74-4ab0-a0ec-23142066c01e" />

**4. Criar a subpasta "Links importantes"**

<img width="720" height="414" alt="Subpasta Links importantes" src="https://github.com/user-attachments/assets/06b27a5d-1644-419f-af76-6852b3630dcf" />

**5. Criar a subpasta "Comandos Básicos"**

<img width="658" height="240" alt="Subpasta Comandos Básicos" src="https://github.com/user-attachments/assets/944c7898-7732-4367-9a3a-69d2a5c34258" />

**6. Dentro de "Comandos Básicos", criar o primeiro notebook**

<img width="871" height="503" alt="Criar primeiro notebook" src="https://github.com/user-attachments/assets/96095b37-9408-4140-9ccc-b331f9822fed" />

**7. Renomear o notebook para "testes"**

<img width="753" height="364" alt="Renomear notebook" src="https://github.com/user-attachments/assets/3bfffc5f-73a9-4c09-a0bf-a46eb213deda" />

**8. Remover o notebook de testes**

<img width="1108" height="581" alt="Remover notebook de testes" src="https://github.com/user-attachments/assets/6f481ac5-feb5-4c12-af0f-a3bf43fbadf4" />

---

## Parte 2 — Hierarquia do Unity Catalog

> **Resumo:** o Unity Catalog é a solução de governança unificada de dados e IA da Databricks — uma camada única de controle aplicada a todos os workspaces, em qualquer nuvem (AWS, Azure ou GCP).

### A hierarquia em um diagrama

```mermaid
graph TD
    C["📦 Catalog<br/>nível mais alto · governança e isolamento<br/>(ex: prod, dev)"]
    C --> S["📁 Schema<br/>agrupamento lógico por contexto/projeto"]
    S --> T["📊 Tabelas<br/>dados estruturados (Delta Lake)<br/>consultas SQL, análises, ML"]
    S --> V["🗂️ Volumes<br/>dados não estruturados<br/>imagens, PDFs, JSONs, CSVs, logs"]
```

### O que é cada nível

| Nível | Função principal | Tipo de dado contido |
|---|---|---|
| **Catalog** | Governança e isolamento de alto nível | Schemas |
| **Schema** | Agrupamento lógico por contexto/projeto | Tabelas, Views, Volumes |
| **Tabelas** | Dados estruturados para consulta SQL/Analytics | Linhas e colunas (Delta) |
| **Volumes** | Armazenamento governado de arquivos brutos | Arquivos não estruturados |

<img width="546" height="411" alt="Hierarquia Unity Catalog" src="https://github.com/user-attachments/assets/9464e199-d287-4bb8-938f-139820b77202" />
<img width="1343" height="419" alt="Hierarquia Unity Catalog detalhada" src="https://github.com/user-attachments/assets/e3406691-85d2-4186-bb05-2e210ce09267" />

### Passo a passo: criando a hierarquia na prática

**1. Escolher a linguagem da célula e criar novas células de código**

<img width="1259" height="521" alt="Escolher linguagem" src="https://github.com/user-attachments/assets/925eed28-03cd-434c-bf99-2d813997a3c1" />
<img width="1108" height="470" alt="Criar novas células" src="https://github.com/user-attachments/assets/079b4e74-ea59-4cb1-a929-97464e45b5e9" />

**2. Criar um Catalog** (exemplo baseado na [documentação oficial da Microsoft](https://learn.microsoft.com/en-us/azure/databricks/catalogs/create-catalog))

<img width="715" height="149" alt="Comando criar catalog" src="https://github.com/user-attachments/assets/8c0e4ae0-768b-4e64-be07-e4e556ebf41b" />
<img width="752" height="457" alt="Executando criação do catalog" src="https://github.com/user-attachments/assets/c5c3dc1d-2b43-4de9-9055-0f6be746d0de" />

Após executar o comando na célula, o Catalog é criado:

<img width="545" height="380" alt="Catalog criado" src="https://github.com/user-attachments/assets/be4e609d-1763-4fc5-bf30-8ee4d9c0ddb3" />

**3. Deletar um Catalog**

<img width="714" height="139" alt="Comando deletar catalog" src="https://github.com/user-attachments/assets/0677952c-4abd-4466-9861-4f8a1a35a645" />
<img width="707" height="175" alt="Confirmação de exclusão" src="https://github.com/user-attachments/assets/96be1e79-225b-43e7-89d4-2d9795990041" />

**4. Criar a hierarquia completa (Catalog → Schema → Tabela/Volume)**

<img width="706" height="324" alt="Hierarquia completa" src="https://github.com/user-attachments/assets/1820eca4-9b73-4278-b640-02aa101f6100" />
<img width="711" height="122" alt="Comando hierarquia" src="https://github.com/user-attachments/assets/1d7dde29-a06e-40d7-8119-e8b07a6676fa" />
<img width="1080" height="407" alt="Executando hierarquia" src="https://github.com/user-attachments/assets/c044df4a-fe87-4a4a-8204-23c75dd91c4e" />

**5. Criar o Schema**

<img width="1325" height="486" alt="Criando schema" src="https://github.com/user-attachments/assets/67aeff62-f1eb-43fb-a9b4-963073129abc" />

> Dentro do Schema, é possível criar **Volumes** e **Tabelas**.

**6. Criar uma Tabela**

<img width="1055" height="245" alt="Criação de tabela" src="https://github.com/user-attachments/assets/d5dfd2a8-5c28-4d4b-9029-12c3e7beb4b7" />

**7. Criar um Volume**

<img width="1089" height="498" alt="Criação de volume" src="https://github.com/user-attachments/assets/554313ca-6d6b-485e-a718-2e7d81c1c146" />

**8. Resultado final** — Tabela e Volume dentro do mesmo Schema:

<img width="1100" height="445" alt="Tabela e volume no schema" src="https://github.com/user-attachments/assets/71520680-b433-4718-983a-6f275a052999" />

---

[Criando ambiente Oficial do Curso](https://github.com/joycequoos/Databricks_Ambiente_Oficial_Curso)


**Próximos passos:** aprofundar em manipulação de dados com PySpark (filtros, colunas, tipos, agregações) dentro dessa estrutura já organizada.
