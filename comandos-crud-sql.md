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








