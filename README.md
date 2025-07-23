# dashboard-star-schema
Projeto DIO Dashboard de Vendas com Power BI utilizando Star Schema

# Esquema em Estrela para Análise de Dados de Professores (Universidade)

Este documento descreve um esquema em estrela projetado para facilitar a análise de dados relacionados aos professores de uma universidade, seus cursos, departamentos e informações temporais. 

O objetivo é fornecer uma estrutura otimizada para consultas, relatórios e análise de tendências utilizando ferramentas de Business Intelligence (BI).


<img width="925" height="537" alt="image" src="https://github.com/user-attachments/assets/937c33eb-cf1d-4acd-9b00-4780035b0630" />


## Visão Geral

O esquema em estrela é composto por uma tabela fato central (`Fato_Professor`) e várias tabelas dimensão que fornecem contexto detalhado sobre professores, cursos, departamentos e datas.

## Tabelas

### Tabela Fato: `Fato_Professor`

A tabela fato centraliza as principais métricas e ligações para as tabelas dimensão.

*   **idFatoProfessor** (INT, Chave Primária): Identificador único para cada registro na tabela fato.
*   **idProfessor** (INT, Chave Estrangeira): Referência ao ID do professor na tabela `Dim_Professor`.
*   **idCurso** (INT, Chave Estrangeira): Referência ao ID do curso na tabela `Dim_Curso`.
*   **idDepartamento** (INT, Chave Estrangeira): Referência ao ID do departamento na tabela `Dim_Departamento`.
*   **idData** (INT, Chave Estrangeira): Referência ao ID da data na tabela `Dim_Data`.
*   **Numero_Alunos_Matriculados** (INT): Número de alunos matriculados em cursos ministrados pelo professor. (Métrica)

### Tabelas Dimensão

1.  **`Dim_Professor`**

    *   **idProfessor** (INT, Chave Primária): ID único do professor.
    *   **Nome_Professor** (VARCHAR(45)): Nome do professor.

2.  **`Dim_Curso`**

    *   **idCurso** (INT, Chave Primária): ID único do curso.
    *   **Nome_Curso** (VARCHAR(45)): Nome do curso.

3.  **`Dim_Departamento`**

    *   **idDepartamento** (INT, Chave Primária): ID único do departamento.
    *   **Nome_Departamento** (VARCHAR(45)): Nome do departamento.
    *   **Campus** (VARCHAR(45)): Localização do campus.

4.  **`Dim_Data`**

    *   **idData** (INT, Chave Primária): ID único da data.
    *   **Data** (DATE): Data completa.
    *   **Dia** (INT): Dia do mês.
    *   **Mes** (INT): Mês do ano.
    *   **Ano** (INT): Ano.
    *   **Trimestre** (INT): Trimestre do ano (1, 2, 3, 4).
    *   **Semestre** (INT): Semestre do ano (1, 2).

## Relacionamentos

*   `Fato_Professor` (idProfessor) --> `Dim_Professor` (idProfessor)
*   `Fato_Professor` (idCurso) --> `Dim_Curso` (idCurso)
*   `Fato_Professor` (idDepartamento) --> `Dim_Departamento` (idDepartamento)
*   `Fato_Professor` (idData) --> `Dim_Data` (idData)

## Passo a Passo para Implementação e Uso

1.  **Criação das Tabelas:**

    *   Utilize um sistema de gerenciamento de banco de dados relacional (SGBDR) como MySQL, PostgreSQL ou SQL Server.
    *   Crie as tabelas `Fato_Professor`, `Dim_Professor`, `Dim_Curso`, `Dim_Departamento` e `Dim_Data` com as colunas e tipos de dados especificados acima.
    *   Defina as chaves primárias e estrangeiras para garantir a integridade referencial.
    *   Considere a criação de índices nas chaves estrangeiras para otimizar o desempenho das consultas.

2.  **Carga de Dados:**

    *   Extraia os dados das tabelas relacionais existentes (original da imagem) ou de outras fontes de dados.
    *   Transforme os dados para adequá-los ao formato do esquema em estrela. Isso pode envolver a criação de novas colunas, a limpeza de dados inconsistentes e a normalização de valores.
    *   Carregue os dados transformados nas tabelas do esquema em estrela.  Comece pelas tabelas de dimensão (`Dim_Professor`, `Dim_Curso`, `Dim_Departamento`, `Dim_Data`) e, em seguida, carregue a tabela fato (`Fato_Professor`).

3.  **Consultas e Análise:**

    *   Escreva consultas SQL para extrair informações relevantes. Exemplo:

        ```sql
        SELECT
            dp.Nome_Departamento,
            AVG(fp.Numero_Alunos_Matriculados) AS Media_Alunos
        FROM
            Fato_Professor fp
        JOIN
            Dim_Professor pr ON fp.idProfessor = pr.idProfessor
        JOIN
            Dim_Departamento dp ON fp.idDepartamento = dp.idDepartamento
        JOIN
            Dim_Data dt ON fp.idData = dt.idData
        WHERE
            dt.Ano = YEAR(CURDATE())  -- Ano atual
        GROUP BY
            dp.Nome_Departamento
        ORDER BY
            Media_Alunos DESC;
        ```

    *   Utilize ferramentas de BI como Power BI, Tableau ou Qlik Sense para criar relatórios e dashboards interativos. Conecte as tabelas do esquema em estrela na ferramenta BI e explore os dados visualmente.

4.  **Análise Temporal:**

    *   Utilize a tabela `Dim_Data` para segmentar as análises por ano, trimestre, mês, etc., e identificar tendências ao longo do tempo.
    *   Crie gráficos de linha para visualizar a evolução de métricas como o número de alunos matriculados ao longo do tempo.

5.  **Manutenção:**

    *   Implemente processos de ETL (Extract, Transform, Load) automatizados para manter os dados do esquema em estrela atualizados regularmente.
    *   Monitore o desempenho das consultas e crie índices adicionais conforme necessário.
    *   Revise o esquema em estrela periodicamente para garantir que ele atenda às necessidades de análise em evolução.

## Considerações

*   **Granularidade:** A granularidade da tabela fato é importante. No exemplo, cada linha representa um professor, curso e departamento. Ajuste a granularidade conforme necessário para atender aos requisitos de análise.
*   **Métricas:** As métricas na tabela fato são os valores que você deseja analisar. No exemplo, é o número de alunos matriculados. Adicione outras métricas relevantes conforme necessário.
*   **Chaves:** As chaves estrangeiras são essenciais para ligar a tabela fato às tabelas dimensão. Garanta a integridade referencial para evitar inconsistências nos dados.
*   **Dados Historicos**: Se houver necessidade de guardar dados historicos, considere o uso de tabelas de Dimensão com versionamento dos registros.

## Exemplo de Cenários de Análise

*   Identificar os departamentos com o maior número médio de alunos matriculados por professor.
*   Analisar a evolução do número de alunos matriculados ao longo do tempo, segmentado por departamento.
*   Comparar o desempenho de diferentes professores em termos de número de alunos matriculados.
*   Identificar os cursos com o maior número de alunos matriculados.
*   Criar relatórios para acompanhar o desempenho dos professores e departamentos ao longo do tempo.

Este README fornece um guia detalhado para a implementação e uso de um esquema em estrela para análise de dados de professores em um contexto universitário.
