# Comandos SQL para CRUD - Referência

## Resumo
- C -> Create (inserir dados)
- R -> Read (ler dados)
- U -> Update (atualizar dados)
- D -> Delete (excluir Dados)

## INSERT

### Fabricantes
```sql
INSERT INTO fabricantes (nome) VALUES('Asus');
INSERT INTO fabricantes (nome) VALUES('Dell');

INSERT INTO fabricantes (nome) 
VALUES('Apple'), ('LG'), ('Samsung'), ('Brastemp');

INSERT INTO fabricantes (nome) 
VALUES('Positivo'), ('Microsoft');
```

### Produtos

```sql
INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Ultrabook',
    'Ultrabook Asus com processador Intel Core i12, memória RAM de 16GB e Windows 11',
    6500.99,
    7,
    1
);

INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Tablet Android',
    'Tablet com a versão 12 do sistema operacional da Google, possui tela de 10 polegadas e armazenamento de 64GB.',
    4999,
    3,
    5 # Samsung
);

INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES
(
    'Geladeira',
    'Refrigerador Frost-free com acesso à Internet das Coisas e bla bla bla',
    1500,
    10,
    6 -- Brastemp
),
(
    'iPhone 13 Pro Max',
    'Alta durabilidade, processador Bionic 14, 128 GB de armazenamento, 6GB de RAM e caro pra caramba',
    6999.99,
    3,
    3 -- Apple
),
(
    'iPad Mini',
    'Tablet Apple com tela retina display de 4k, memória interna de 64GB, acesso ao iCloud.',
    5000,
    8,
    3 -- Apple
);

INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Xbox',
    'Console de última geração com acesso aos melhores jogos e bla bla',
    2500,
    6,
    8
);

INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Ultrabook',
    'Equipamento com processador AMD Ryzen5, 12GB de RAM, placa de vídeo RTX',
    4500.68,
    12,
    7
);

```


## SELECT

### Ler dados da tabela produtos
```sql
SELECT * FROM produtos;

SELECT nome, preco FROM produtos;

SELECT preco, nome FROM produtos WHERE preco < 5000;

SELECT nome, descricao FROM produtos 
WHERE fabricante_id = 3; # Apple
```

### Operadores Lógicos: E OU NÃO
```sql
SELECT * FROM produtos 
WHERE preco > 5000 AND preco < 8000;

SELECT nome, preco FROM produtos
-- WHERE fabricante_id = 3 OR fabricante_id = 8;
WHERE fabricante_id IN(3, 8); -- usando função IN(lista)

--Monte uma consulta que traga nome, preco e quantidade
--de todos os produtos exceto os do fabricante apple

SELECT nome, preco, quantidade FROM produtos
-- WHERE NOT fabricante_id = 3; # versão 1 usando NOT
WHERE fabricante_id != 3; -- versão 2 usando operador !=

```

### Filtros
```sql
SELECT nome, preco FROM produtos ORDER BY nome; -- ASC
SELECT nome, preco FROM produtos ORDER BY nome DESC;

-- ASC -> ordenação em modo crescente (padrão)
-- DESC -> ordenação em modo decrescente

SELECT nome, descricao FROM produtos
WHERE descricao LIKE '%processador%'; -- LIKE (COMO)
-- % OPERADOR CORINGA (SIGNIFICA QUALQUER TEXTO)
```

### Operações e Funções de agregação
```sql
SELECT SUM(preco) FROM produtos; -- SOMA

SELECT SUM(preco) AS TOTAL -- AS É UM ALIAS (APELIDO)
FROM produtos;

SELECT SUM(quantidade) AS "Quantidade em Estoque"
FROM produtos;

SELECT SUM(quantidade) AS "Quantidade em Estoque"
FROM produtos WHERE fabricante_id = 3; -- Apple

-- AVG (AVERAGE) MÉDIA
SELECT AVG(preco) AS "Média dos Preços" -- Apelido
FROM produtos;

-- ROUND (Arredondamento)
SELECT ROUND(AVG(preco), 2) AS "Média dos Preços"
FROM produtos;

-- COUNT (Contagem)
SELECT COUNT(id) AS "Qtd de Produtos"
FROM produtos;

-- DISTINCT é um comando para evitar a duplicidade na contagem de campos que não são chave-primária
SELECT COUNT(DISTINCT fabricante_id) 
AS "Qtd de Fabricantes" FROM produtos;

SELECT nome, preco, quantidade, 
(preco * quantidade) AS Total
FROM produtos;
```

### Agrupamentos
```sql
SELECT fabricante_id, SUM(preco) AS Total FROM produtos
GROUP BY fabricante_id;

--GROUP BY permite segmentar resultados da consulta. Neste caso, somamos todos os preços e segmentamos/agrupamos por cada fabricante.
```


## UPDATE (sempre com WHERE)

### Atualizar dados de uma tabela
```sql
UPDATE fabricantes SET nome = 'Microsoft Brasil'
WHERE id = 8;

-- Mudar o preço do Ultrabook da Positivo para 5200.
UPDATE produtos SET preco = 5200
WHERE id = 7;

-- Mudar a quantidade dos produtos da Asus e da Apple para 15.
UPDATE produtos SET quantidade = 15
WHERE fabricante_id = 1 OR fabricante_id = 3; 
```

### Excluir dados de uma tabela
```sql
DELETE FROM fabricantes WHERE id = 4; -- LG

DELETE FROM produtos
WHERE preco <= 2000 AND preco > 500;
```

### Consultas em duas ou mais tabelas (JUNÇÃO)
```sql
-- SELECT nomeDaTabela.nomeDaColuna
SELECT produtos.nome, fabricantes.nome

-- INNER JOIN é o comando que permite JUNTAR tabelas
FROM produtos INNER JOIN fabricantes

-- ON comando para indicar o critério da junção
ON produtos.fabricante_id = fabricantes.id;


SELECT produtos.nome, fabricantes.nome
FROM produtos INNER JOIN fabricantes
ON produtos.fabricante_id = fabricantes.id;

-- Nome do Produto e do Fabricante, ordenados pelo nome do produto
SELECT 
    produtos.nome AS Produto, 
    fabricantes.nome AS Fabricante
FROM produtos INNER JOIN fabricantes
ON produtos.fabricante_id = fabricantes.id
ORDER BY produtos.nome;

-- Fabricante, soma dos preços e quantidade de produtos
SELECT 
    fabricantes.nome AS Fabricante,
    SUM(produtos.preco) AS Total,
    COUNT(produtos.fabricante_id) AS "Qtd de Produtos"
FROM produtos INNER JOIN fabricantes
ON produtos.fabricante_id = fabricantes.id
GROUP BY Fabricante
ORDER BY Total;

-- Trazer a quantidade de produtos de cada fabricante

-- INNER JOIN traz os registros somente daqueles fabricantes que tem produtos
SELECT 
    fabricantes.nome AS Fabricante,
    COUNT(produtos.id) AS "Quantidade de Produtos"
FROM produtos INNER JOIN fabricantes
ON produtos.fabricante_id = fabricantes.id
GROUP BY Fabricante;

-- RIGHT/LEFT JOIN traz os registros mesmo daqueles fabricantes que não tem produtos
SELECT 
    fabricantes.nome AS Fabricante,
    COUNT(produtos.id) AS "Quantidade de Produtos"
FROM produtos RIGHT JOIN fabricantes
ON produtos.fabricante_id = fabricantes.id
GROUP BY Fabricante;
```












