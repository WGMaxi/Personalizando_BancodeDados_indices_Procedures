# Personalizando_BancodeDados_indices_Procedures
Criação de índices para consultas para o cenário de company com as perguntas (queries sql) para recuperação de informações. Sendo assim, dentro do script será criado os índices com base na consulta SQL.  <br/>
- Qual o departamento com maior número de pessoas?
SELECT department_id, COUNT(*) AS numero_pessoas <br/>
FROM employees <br/>
GROUP BY department_id <br/>
ORDER BY numero_pessoas DESC <br/>
LIMIT 1; <br/>
-Quais são os departamentos por cidade?
SELECT city, department_id, COUNT(*) AS numero_pessoas<br/>
FROM employees<br/>
JOIN departments ON employees.department_id = departments.department_id<br/>
GROUP BY city, department_id;<br/>
-Relação de empregados por departamento
SELECT department_id, employee_id, first_name, last_name<br/>
FROM employees<br/>
ORDER BY department_id;<br/>
## Criação de Índices
-- Índice para acelerar a busca por departamentos na tabela employees<br/>
CREATE INDEX idx_department_id ON employees(department_id);<br/>
-- Índice para acelerar a busca por cidade na tabela employees<br/>
CREATE INDEX idx_city ON employees(city);<br/>

-Tipo de Índice e Justificativa:
Índice idx_department_id:<br/>

Tipo: Índice B-Tree (padrão);<br/>
Justificativa: A busca por departamento é comum nas consultas. O índice B-Tree é adequado para consultas de igualdade (como na cláusula WHERE department_id = x) e eficiente para este cenário.<br/>
Índice idx_city:<br/>

Tipo: Índice B-Tree (padrão);<br/>
Justificativa: A busca por cidade também é comum nas consultas. O índice B-Tree é eficaz para consultas de igualdade.<br/>
## Utilização de Procedures para Manipulação de Dados em Banco de Dados

DELIMITER //<br/>

CREATE PROCEDURE ManipularDadosEcommerce(<br/>
    IN p_opcao INT,<br/>
    IN p_produto_id INT,<br/>
    IN p_nome_produto VARCHAR(255),<br/>
    IN p_quantidade INT,<br/>
    IN p_preco DECIMAL(10,2)<br/>
)<br/>
BEGIN<br/>
    CASE p_opcao<br/>
        WHEN 1 THEN<br/>
            -- Instruções para INSERIR novo produto
            INSERT INTO produtos (nome_produto, quantidade, preco)<br/>
            VALUES (p_nome_produto, p_quantidade, p_preco);<br/>
        WHEN 2 THEN<br/>
            -- Instruções para ATUALIZAR informações do produto<br/>
            UPDATE produtos <br/>
            SET nome_produto = p_nome_produto, <br/>
                quantidade = p_quantidade, <br/>
                preco = p_preco <br/>
            WHERE produto_id = p_produto_id;<br/>
        WHEN 3 THEN<br/>
            -- Instruções para REMOVER produto<br/>
            DELETE FROM produtos WHERE produto_id = p_produto_id;<br/>
        ELSE<br/>
            SELECT 'Opção inválida';<br/>
    END CASE;<br/>
END //<br/>

DELIMITER ;<br/>



