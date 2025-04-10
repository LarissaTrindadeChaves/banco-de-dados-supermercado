# banco-de-dados-supermercado

create database supermercadosalvatore;
use supermercadosalvatore;

create table cliente (
    id_cliente integer primary key not null unique auto_increment,
    nome varchar(100) not null,
    cpf varchar(45) not null unique,
    telefone varchar(20) not null,
    email varchar(250) not null
); 

create table colaborador (
    id_colaborador integer primary key not null unique auto_increment,
    nome varchar(100) not null,
    cargo varchar(80) not null,
    telefone varchar(20) not null,
    data_admissao date not null,
    salario decimal not null,
    cpf varchar(45) not null unique
);

create table fornecedores (
    id_fornecedores integer primary key not null unique auto_increment,
    nome varchar(100) not null,
    telefone varchar(20) not null,
    cnpj varchar(30) not null unique
);

create table promocao (
    id_promocao integer primary key not null unique auto_increment,
    nome varchar(100) not null,
    data_inicio date not null,
    data_fim date not null
);

create table endereco (
    id_endereco integer primary key not null unique auto_increment,
    logradouro varchar(150),
    bairro varchar(45),
    cep varchar(45),
    cidade varchar(45),
    id_cliente integer not null,
    foreign key(id_cliente) references cliente(id_cliente)
);

create table venda(
    id_venda integer primary key not null unique auto_increment,
    valor_total float not null,
    id_cliente integer not null,
    id_colaborador integer not null,
    foreign key (id_cliente) references cliente(id_cliente),
    foreign key (id_colaborador) references colaborador(id_colaborador)
);

create table pagamento(
    id_pagamento integer primary key not null unique auto_increment,
    forma_pagamento varchar(50) not null,
    valor float not null,
    id_venda integer not null,
    foreign key (id_venda) references venda(id_venda)
);

create table devolucao(
    id_devolucao integer primary key not null unique auto_increment,
    data_devolucao date not null,
    motivo text,
    id_venda integer not null,
    foreign key (id_venda) references venda(id_venda)
);

create table produto(
    id_produto integer primary key not null unique auto_increment,
    nome varchar(100) not null,
    codigo_produto varchar(80) not null unique,
    categoria varchar(50) not null,
    estoque integer not null,
    preco float not null,
    id_fornecedores integer not null,
    foreign key (id_fornecedores) references fornecedores(id_fornecedores)
);

create table itens_venda(
    id_itens_venda integer primary key not null unique auto_increment,
    qtde integer not null,
    preco_unitario float not null,
    id_venda integer not null,
    id_produto integer not null,
    foreign key (id_venda) references venda(id_venda),
    foreign key (id_produto) references produto(id_produto)
);

create table produtos_promocoes(
    id_produtos_promocoes integer primary key not null unique auto_increment,
    desconto decimal not null,
    id_produto integer not null,
    id_promocao integer not null,
    foreign key (id_produto) references produto(id_produto),
    foreign key (id_promocao) references promocao(id_promocao)
);

insert into cliente (nome, cpf, telefone, email)
values
    ('Arthur Pereira Kohl', '123.456.789-00', '(21) 98765-4321', 'arthur@gmail.com'),
    ('Isabella Trindade Chaves', '987.654.321-00', '(21) 97654-3210', 'isabellatrinity@gmail.com'),
    ('Jadir Alberto Chaves', '456.123.789-00', '(22) 98547-6543', 'jackchaves@gmail.com'),
    ('Elizangela Trindade', '789.456.123-00', '(24) 99874-2365', 'elizangela@gmail.com'),
    ('Leila Machado', '321.654.987-00', '(21) 96587-1234', 'leila@gmail.com');

insert into colaborador (nome, cargo, telefone, data_admissao, salario, cpf)
values
    ('Vicki Donovan', 'Caixa', '(21) 99876-5432', '2023-05-12', 2500.00, '159.753.468-00'),
    ('Matt Donovan', 'Gerente', '(21) 98456-7890', '2022-02-20', 4800.00, '357.951.852-00'),
    ('Tyler Lockwood', 'Repositor', '(22) 99632-1478', '2023-08-14', 2100.00, '654.852.741-00');

insert into fornecedores (nome, telefone, cnpj)
values
    ('Distribuidora Alimentar RJ', '(21) 2567-8900', '12.345.678/0001-90'),
    ('Fornecedora Bebidas Ltda', '(22) 3698-4521', '23.456.789/0001-22'),
    ('Hortifruti Natural', '(21) 2548-9632', '34.567.890/0001-55');

insert into promocao (nome, data_inicio, data_fim)
values
    ('Promoção Semana do Consumidor', '2025-03-10', '2025-03-17'),
    ('Festival de Inverno', '2025-07-15', '2025-07-31');

insert into endereco (logradouro, bairro, cep, cidade, id_cliente)
values
    ('Rua das Flores, 123', 'Centro', '20031-140', 'Rio de Janeiro', 1),
    ('Avenida Brasil, 4500', 'Bonsucesso', '21030-900', 'Rio de Janeiro', 2),
    ('Rua XV de Novembro, 75', 'Centro', '28625-500', 'Teresópolis', 3),
    ('Rua Barão do Amazonas, 300', 'Centro', '25804-000', 'Três Rios', 4),
    ('Rua das Acácias, 87', 'Recreio', '20521-060', 'Rio de Janeiro', 5);

insert into venda (valor_total, id_cliente, id_colaborador)
values
    (50.80, 1, 1),
    (30.00, 2, 2),
    (15.70, 3, 3),
    (98.20, 4, 1),
    (22.50, 5, 2);

insert into pagamento (forma_pagamento, valor, id_venda)
values
    ('cartao_credito', 50.80, 1),
    ('pix', 30.00, 2),
    ('dinheiro', 15.70, 3),
    ('cartao_debito', 98.20, 4),
    ('pix', 22.50, 5);

insert into devolucao (data_devolucao, motivo, id_venda)
values
    ('2025-02-20', 'Produto com defeito', 2),
    ('2025-02-25', 'Cliente desistiu da compra', 4);

insert into produto (nome, codigo_produto, categoria, estoque, preco, id_fornecedores)
values
    ('Arroz 5kg', 'ARZ-001', 'Alimentos', 100, 25.90, 1),
    ('Feijão Preto 1kg', 'FEJ-002', 'Alimentos', 80, 8.50, 1),
    ('Coca-Cola 2L', 'COC-003', 'Bebidas', 120, 9.99, 2),
    ('Barra de Chocolate 150g', 'CHO-004', 'Doces', 90, 7.50, 2),
    ('Maçã Fuji 1kg', 'MAC-005', 'Hortifruti', 60, 6.90, 3),
    ('Tomate Italiano 1kg', 'TOM-006', 'Hortifruti', 75, 4.80, 3);

insert into itens_venda (qtde, preco_unitario, id_venda, id_produto)
values
    (2, 25.90, 1, 1), 
    (1, 9.99, 2, 3),  
    (2, 6.90, 3, 5),  
    (3, 7.50, 4, 4),  
    (3, 4.80, 5, 6); 

insert into produtos_promocoes (desconto, id_produto, id_promocao)
values
    (10.00, 1, 1),
    (5.00, 3, 1),
    (15.00, 4, 2);

select * from cliente;
select * from colaborador;
select * from fornecedores;
select * from promocao;
select * from endereco;
select * from venda;
select * from pagamento;
select * from devolucao;
select * from produto;
select * from itens_venda;
select * from produtos_promocoes;

show tables;
