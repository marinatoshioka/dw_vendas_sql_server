# Projeto de Data Warehouse (DW) para Análise de Vendas

Este projeto foi desenvolvido com o objetivo de demonstrar a construção de um ecossistema de Business Intelligence baseado em SQL Server — desde o banco transacional até o consumo no Power BI.

---

## Objetivo do Projeto

Demonstrar capacidade prática em:

- ETL  
- Construção de Stage  
- Modelagem Dimensional  
- Criação de DataMart/Data Warehouse  
- Cargas automáticas  
- Transformação de dados  
- Integração com Power BI  
- Boas práticas de organização e versionamento  

---

## Arquitetura Geral do Projeto

**BANCO TRANSACIONAL**  
(.bak com dados fictícios de ambiente de homologação — não anexado por questões de tamanho)  
⬇️  
**STAGE (Staging Area)**  
⬇️  
**DATA WAREHOUSE (DW)**  
⬇️  
**POWER BI (Modelo Estrela)**

---

## 1. Stage – Extração e Tratamento Inicial

O processo de carga do Stage contempla:

- Transporte dos dados do banco transacional para o Stage  
- Validação dos schemas  
- Criação automática das tabelas caso sejam excluídas  
- Procedimentos que limpam (TRUNCATE) e repopulam (INSERT INTO) o Stage  
- Preparação dos dados brutos para alimentar dimensões e fato  

### Funcionalidades utilizadas no Stage

- JOINs  
- CTEs  
- Tabelas temporárias  
- Subqueries  
- INSERT / UPDATE  
- MERGE  
- Normalização de dados (ex.: PascalCase e tratamento de nulos com `ISNULL`)  
- Validação automática de schema e recriação de tabelas quando necessário  

### Exemplo real utilizado no projeto

```sql
IF OBJECT_ID('ST_VENDEDORES') IS NULL
BEGIN
    CREATE TABLE ST_VENDEDORES
    (
        COD_VENDEDOR NUMERIC(15),
        NOME VARCHAR(100)
    )
END 
```


## 2. Criação do Data Warehouse (DW)
O Data Warehouse foi desenvolvido seguindo o padrão de modelo estrela, contendo:

Tabelas Dimensão
- D_CLIENTE

- D_EMPRESAS

- D_PRODUTOS

- D_VENDEDORES

- D_CALENDARIO (gerada por função auxiliar)

Tabela Fato
- F_VENDAS

As dimensões foram normalizadas e preparadas para análises:

- Tratamento de campos nulos

- Remoção de duplicidades

- Padronização de texto

Recursos utilizados na construção do DW
MERGE para atualizações inteligentes

- VIEWS de apoio

- Criação de variáveis

- Função para montar D_CALENDARIO

- Procedures automáticas de carga FULL das dimensões

- Procedures de carga incremental da tabela fato

## 3. Automatização da Carga

O projeto inclui:

- Procedures automatizadas para cargas completas e incrementais

- Documentação da estrutura de Jobs no SQL Server Agent
(o agendamento não é executado no ambiente gratuito, mas está descrito conceitualmente)

## 4. Integração com Power BI
- Conexão direta ao DW

- Transformação da tabela D_CALENDARIO no Power Query, tornando-a dinâmica conforme as datas de F_VENDAS

- Modelo estrela completo

- Relacionamentos configurados entre fato e dimensões

- Criação de medidas DAX

- Visualizações analíticas (em desenvolvimento)

- O arquivo .pbix será disponibilizado na pasta /powerbi

## Estrutura do Repositório
/scripts_stage  → Scripts SQL com criação e carga do transacional para Stage; criação de views e tratamentos.
/scripts_dw     → Scripts SQL das dimensões, fatos, merges e tabela calendário.
/powerbi        → Arquivo .pbix com o modelo dimensional.
/diagramas      → Diagramas do modelo (opcional).
