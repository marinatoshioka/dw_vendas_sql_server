📌 Projeto de Data Warehouse (DW) para Análise de Vendas



Este projeto foi desenvolvido com o objetivo de demonstrar a construção de um ecossistema de Business Intelligence baseado em SQL Server — desde o banco transacional até o consumo no Power BI.



🎯 Objetivo do Projeto



Demonstrar capacidade prática em:



* ETL 



* Construção de Stage



* Modelagem Dimensional



* Criação de DataMart/Data Warehouse



* Cargas automáticas



* Transformação de dados



* Integração com Power BI



* Boas práticas de organização e versionamento









📌 Arquitetura Geral do Projeto



BANCO TRANSACIONAL (.bak com dados de ambiente de homologação/fictícios, não anexado por questões de tamanho)

&nbsp;       │

&nbsp;       ▼

&nbsp;    STAGE (Staging Area)

&nbsp;       │

&nbsp;       ▼

&nbsp;DATA WAREHOUSE (DW)

&nbsp;       │

&nbsp;       ▼

&nbsp;  POWER BI (Modelo Estrela)







1\. Stage – Extração e Tratamento Inicial



Processo de carga do Stage contempla:



* Transporte dos dados do banco transacional para o Stage



* Validação dos schemas



* Criação automática das tabelas caso sejam excluídas



* Procedimentos que limpam (TRUNCATE) e repopulam (INSERT INTO) o Stage



* Preparação dos dados brutos para alimentar dimensões e fato





Funcionalidades utilizadas no Stage:



* JOINs



* CTEs



* Tabelas temporárias



* Subqueries



* INSERT / UPDATE



* MERGE



* Normalização de dados (ex.: PascalCase, tratamento de nulos - ISNULL)



* Validação automática de schema e recriação de tabelas do Stage quando necessário. 



Exemplo real utilizado no projeto para garantir que a tabela do Stage (neste caso, a D\_Vendedores) exista, recriando-a automaticamente caso tenha sido excluída:



IF OBJECT\_ID('ST\_VENDEDORES') IS NULL

BEGIN

&nbsp;   CREATE TABLE ST\_VENDEDORES

&nbsp;   (

&nbsp;       COD\_VENDEDOR NUMERIC(15),

&nbsp;       NOME VARCHAR(100)

&nbsp;   )

END





2\. Criação do Data Warehouse (DW)





O DW segue o padrão de modelo estrela, contendo:





Tabelas Dimensão



D\_CLIENTE



D\_EMPRESAS



D\_PRODUTOS



D\_VENDEDORES



D\_CALENDARIO (gerada por função auxiliar)





Tabela Fato



F\_VENDAS





As dimensões foram normalizadas e preparadas para análises:



* Tratamento de campos nulos



* Remoção de duplicidades



* Padronização de texto





Recursos utilizados na construção do DW:



* MERGE para atualizações inteligentes



* VIEWS de apoio



* Criação de variáveis



* Função para montar D\_CALENDARIO



* Procedures automáticas de carga FULL das dimensões



* Procedures de carga incremental para a fato





3\. Automatização da Carga:



O projeto inclui:



* Procedures automatizadas para cargas completas e incrementais



* SQL Server Agent para agendamento dos Jobs que, por requerer versão paga, o processo é documentado conceitualmente.





4\. Integração com Power BI



* O DW foi conectado posteriormente ao Power BI, onde houve, através do Power Query, transformação da D\_Calendário para torná-la dinâmica, a depender das datas do campo Movimento da tabela F\_vendas



* Modelo estrela completo



* Relacionamentos corretos entre fato e dimensões



* Criação de medidas DAX



* Visualizações analíticas (em desenvolvimento)



* O arquivo .pbix será adicionado na pasta /powerbi.



Estrutura do Repositório



/scripts\_stage – Scripts SQL com criação e carga do transacional para Stage e criação de views. Tratamento dos dados.

/scripts\_dw – Scripts SQL de tabelas de dimensões e fatos vindas do Stage. Merge das tabelas. D\_Calendário. 

/powerbi – Arquivo .pbix com o modelo dimensional  

/diagramas – Diagramas do modelo  





