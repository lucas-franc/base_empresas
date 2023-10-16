# produtos

Reproduza a base de dados apresentada no slide 21 desta tarefa

```
create table marcas (
mrc_id int auto_increment primary key,
mrc_nome varchar(50) not null,
mrc_nacionalidade varchar(50)
);

create table produtos (
prd_id int auto_increment primary key,
prd_nome varchar(50) not null,
prd_qtd_estoque int not null default 0,
prd_estoque_min int not null default 0,
prd_data_fabricacao timestamp default now(),
prd_perecivel boolean,
prd_valor decimal(10,2),
prd_marca_id int,
constraint fk_marca foreign key (prd_marca_id) references marcas(mrc_id)
);

create table fornecedores (
frn_id int auto_increment primary key,
frc_nome varchar(50) not null,
frc_email varchar(50)

);

create table produto_fornecedor (
    pf_prod_id int references produtos (prd_id),
    pf_forn_id int references fornecedores (frn_id)
);

INSERT INTO marcas (mrc_nome, mrc_nacionalidade) VALUES
('Marca A', 'Nacionalidade A'),
('Marca B', 'Nacionalidade B'),
('Marca C', 'Nacionalidade C'),
('Marca D', 'Nacionalidade D'),
('Marca E', 'Nacionalidade E'),
('Marca F', 'Nacionalidade F'),
('Marca G', 'Nacionalidade G'),
('Marca H', 'Nacionalidade H'),
('Marca I', 'Nacionalidade I'),
('Marca J', 'Nacionalidade J');

INSERT INTO produtos (prd_nome, prd_qtd_estoque, prd_estoque_min, prd_perecivel, prd_valor, prd_marca_id) VALUES
('Produto 1', 100, 10, 1, 50.00, 1),
('Produto 2', 50, 5, 0, 30.00, 2),
('Produto 3', 200, 20, 1, 70.00, 1),
('Produto 4', 75, 8, 0, 40.00, 3),
('Produto 5', 30, 3, 1, 60.00, 4),
('Produto 6', 150, 15, 0, 35.00, 5),
('Produto 7', 80, 10, 1, 45.00, 6),
('Produto 8', 90, 12, 0, 55.00, 7),
('Produto 9', 120, 10, 1, 65.00, 8),
('Produto 10', 60, 6, 0, 70.00, 9);

INSERT INTO fornecedores (frc_nome, frc_email) VALUES
('Fornecedor A', 'fornecedor_a@example.com'),
('Fornecedor B', 'fornecedor_b@example.com'),
('Fornecedor C', 'fornecedor_c@example.com'),
('Fornecedor D', 'fornecedor_d@example.com'),
('Fornecedor E', 'fornecedor_e@example.com'),
('Fornecedor F', 'fornecedor_f@example.com'),
('Fornecedor G', 'fornecedor_g@example.com'),
('Fornecedor H', 'fornecedor_h@example.com'),
('Fornecedor I', 'fornecedor_i@example.com'),
('Fornecedor J', 'fornecedor_j@example.com');


INSERT INTO produto_fornecedor (pf_prod_id, pf_forn_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);
```

Crie uma view que mostra todos os produtos e suas respectivas marcas

```
CREATE VIEW ViewProdutosEMarcas AS
SELECT
    p.prd_id,
    p.prd_nome,
    p.prd_qtd_estoque,
    p.prd_estoque_min,
    p.prd_data_fabricacao,
    p.prd_perecivel,
    p.prd_valor,
    m.mrc_nome AS marca
FROM produtos p
INNER JOIN marcas m ON p.prd_marca_id = m.mrc_id;

SELECT * FROM ViewProdutosEMarcas;
```

Crie uma view que mostra todos os produtos e seus respectivos fornecedores

```
CREATE VIEW ViewProdutosEFornecedores AS
SELECT
    p.prd_id,
    p.prd_nome,
    p.prd_qtd_estoque,
    p.prd_estoque_min,
    p.prd_data_fabricacao,
    p.prd_perecivel,
    p.prd_valor,
    f.frc_nome AS fornecedor
FROM produtos p
INNER JOIN produto_fornecedor pf ON p.prd_id = pf.pf_prod_id
INNER JOIN fornecedores f ON pf.pf_forn_id = f.frn_id;

SELECT * FROM ViewProdutosEFornecedores;
```

Crie uma view que mostra todos os produtos e seus respectivos fornecedores e
marcas

```
CREATE VIEW ViewProdutosFornecedoresEMarcas AS
SELECT
    p.prd_id,
    p.prd_nome,
    p.prd_qtd_estoque,
    p.prd_estoque_min,
    p.prd_data_fabricacao,
    p.prd_perecivel,
    p.prd_valor,
    m.mrc_nome AS marca,
    f.frc_nome AS fornecedor
FROM produtos p
INNER JOIN marcas m ON p.prd_marca_id = m.mrc_id
INNER JOIN produto_fornecedor pf ON pf.pf_prod_id = p.prd_id
INNER JOIN fornecedores f ON pf.pf_forn_id = f.frn_id;

SELECT * FROM ViewProdutosFornecedoresEMarcas;
```

Crie uma view que mostra todos os produtos com estoque abaixo do mínimo

```
CREATE VIEW ViewProdutosAbaixoDoNivelDeEstoque AS SELECT p.prd_nome FROM produtos p WHERE p.prd_qtd_estoque < prd_estoque_min;
SELECT * FROM ViewProdutosAbaixoDoNivelDeEstoque;
```

Adicione o campo data de validade

```
ALTER TABLE produtos
ADD prd_data_validade timestamp;
```

Insira novos produtos com essa informação

```
INSERT INTO produtos (prd_nome, prd_qtd_estoque, prd_estoque_min, prd_perecivel, prd_valor, prd_marca_id, prd_data_validade) VALUES
('Produto 11', 5, 10, 1, 53.00, 1,  "2017-07-23"),
('Produto 12', 3, 5, 1, 5.00, 2,  "2024-07-27"),
('Produto 13', 300, 5, 1, 5.50, 1,  "2022-08-13");
```

Crie uma view que mostra todos os produtos e suas respectivas marcas com
validade vencida

```
CREATE VIEW ViewProdutosVencidos AS
SELECT
    p.prd_id,
	p.prd_nome,
    p.prd_data_validade,
    m.mrc_nome AS marca
FROM produtos p
INNER JOIN marcas m ON p.prd_marca_id = m.mrc_id WHERE p.prd_data_validade < now();

SELECT * FROM ViewProdutosVencidos;
```

Selecionar os produtos com preço acima da média

```
SELECT p.prd_nome, p.prd_valor, AVG(p.prd_valor) FROM produtos p WHERE p.prd_valor;
```


