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
    IN p_idproduto INT,<br/>
    IN p_descrição VARCHAR(255),<br/>
    IN p_quantidade INT,<br/>
    IN p_valor DECIMAL(10,2)<br/>
)<br/>
BEGIN<br/>
    CASE p_opcao<br/>
        WHEN 1 THEN<br/>
            -- Instruções para INSERIR novo produto
            INSERT INTO produto (descrição, quantidade, valor)<br/>
            VALUES (p_descrição, p_quantidade, p_valor);<br/>
        WHEN 2 THEN<br/>
            -- Instruções para ATUALIZAR informações do produto<br/>
            UPDATE produtos <br/>
            SET nome_produto = p_descrição, <br/>
                quantidade = p_quantidade, <br/>
                preco = p_valor <br/>
            WHERE idproduto = p_produto_id;<br/>
        WHEN 3 THEN<br/>
            -- Instruções para REMOVER produto<br/>
            DELETE FROM produtos WHERE idproduto = p_produto_id;<br/>
        ELSE<br/>
            SELECT 'Opção inválida';<br/>
    END CASE;<br/>
END //<br/>

DELIMITER ;<br/>

- Chamada da Procedure
  
-- Exemplo de chamada para INSERIR novo produto<br/>
CALL ManipularDadosEcommerce(1, NULL, 'Novo Produto', 100, 49.99);<br/>

-- Exemplo de chamada para ATUALIZAR informações do produto<br/>
CALL ManipularDadosEcommerce(2, 1, 'Produto Atualizado', 150, 59.99);<br/>

-- Exemplo de chamada para REMOVER produto<br/>
CALL ManipularDadosEcommerce(3, 2, NULL, NULL, NULL);<br/>



