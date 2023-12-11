9)Faça uma consulta capaz de listar os dep_id, nome, salario, e as médias de cada departamento utilizando o windows function; 2583ms nao otimizado, desvio 73,65  --- 989ms otimizado, desvio 37,87

Não otimizado:

SELECT 
    e.dep_id,
    e.nome AS nome_empregado,
    e.salario,
    AVG(e.salario) OVER (PARTITION BY e.dep_id) AS media_salario_departamento
FROM empregados e
JOIN departamentos d ON e.dep_id = d.dep_id;

Otimizado:

WITH media_salario_departamento AS (
    SELECT 
        dep_id,
        AVG(salario) AS media_salario
    FROM 
        empregados
    GROUP BY 
        dep_id
)

SELECT 
    e.dep_id,
    e.nome AS nome_empregado,
    e.salario,
    d.media_salario
FROM 
    empregados e
JOIN 
    media_salario_departamento d ON e.dep_id = d.dep_id;



3)Listar o dep_id, nome e o salario do funcionario com maior salario dentro de cada departamento (usar o with) 2396ms nao otimizado, desvio 83,59 --- 2063ms otimizado, desvio 97,57

Não otimizado:

WITH ranked_employees AS (
    SELECT 
        dep_id,
        nome,
        salario,
        ROW_NUMBER() OVER (PARTITION BY dep_id ORDER BY salario DESC) AS rank_salario
    FROM 
        empregados
)

SELECT 
    dep_id,
    nome,
    salario
FROM 
    ranked_employees
WHERE 
    rank_salario = 1;

Otimizado:

SELECT 
    DISTINCT ON (dep_id) dep_id,
    nome,
    salario
FROM 
    empregados
ORDER BY 
    dep_id, salario DESC;

8)Listar os nomes dos colaboradores com salario maior que a média do seu departamento (dica: usar subconsultas); Mais de 3 horas nao otimizado --- 1146ms otimizado, desvio 29,08

Não otimizado:

SELECT 
    nome,
    salario
FROM empregados e
WHERE salario > (
        SELECT AVG(salario)
        FROM empregados
        WHERE dep_id = e.dep_id
    );

Otimizado:

SELECT 
    e.nome,
    e.salario
FROM 
    empregados e
JOIN (
    SELECT 
        dep_id,
        AVG(salario) AS media_por_departamento
    FROM 
        empregados
    GROUP BY 
        dep_id
) media_dep ON e.dep_id = media_dep.dep_id
WHERE 
    e.salario > media_dep.media_por_departamento;


