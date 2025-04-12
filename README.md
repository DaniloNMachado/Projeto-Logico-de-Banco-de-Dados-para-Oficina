```markdown
# Projeto Lógico de Banco de Dados para Oficina

## Descrição do Projeto

Este projeto consiste na criação do esquema lógico de um banco de dados para o contexto de uma oficina. O objetivo é demonstrar a definição de todas as etapas, desde a concepção do esquema utilizando o modelo relacional até a implementação do banco de dados. O esquema lógico foi desenvolvido com base em um modelo conceitual (Modelo ER) prévio, considerando as entidades e relacionamentos relevantes para a gestão de uma oficina. O projeto inclui a criação do script SQL para a definição do esquema, a inserção de dados para testes e a elaboração de queries SQL complexas para demonstrar a funcionalidade e a capacidade de recuperação de informações do banco de dados.

## Modelo Lógico

O modelo lógico do banco de dados para a oficina é composto pelas seguintes tabelas:

* **Clientes:** Armazena informações sobre os clientes da oficina.
* **Veículos:** Contém detalhes sobre os veículos dos clientes.
* **Serviços:** Registra os diferentes tipos de serviços oferecidos pela oficina.
* **Funcionários:** Mantém informações sobre os funcionários da oficina.
* **Agendamentos:** Registra os agendamentos de serviços para os veículos dos clientes.
* **Peças:** Armazena informações sobre as peças utilizadas nos serviços.
* **OrdensDeServico:** Contém informações sobre as ordens de serviço geradas para os agendamentos.
* **OrdemServicoPecas:** Tabela de junção para o relacionamento muitos-para-muitos entre OrdensDeServico e Peças.

## Diagrama do Modelo Lógico (Descrição Textual)

**Tabela Clientes:**

* `id_cliente` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `nome_cliente` (VARCHAR(100), NOT NULL)
* `endereco_cliente` (VARCHAR(255))
* `telefone_cliente` (VARCHAR(20))
* `email_cliente` (VARCHAR(100))

**Tabela Veículos:**

* `id_veiculo` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `id_cliente` (INT, FOREIGN KEY references Clientes(`id_cliente`), NOT NULL)
* `marca_veiculo` (VARCHAR(50), NOT NULL)
* `modelo_veiculo` (VARCHAR(50), NOT NULL)
* `ano_veiculo` (INT)
* `placa_veiculo` (VARCHAR(10), UNIQUE, NOT NULL)

**Tabela Serviços:**

* `id_servico` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `nome_servico` (VARCHAR(100), NOT NULL)
* `descricao_servico` (TEXT)
* `preco_servico` (DECIMAL(10, 2), NOT NULL)

**Tabela Funcionários:**

* `id_funcionario` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `nome_funcionario` (VARCHAR(100), NOT NULL)
* `cargo_funcionario` (VARCHAR(50))
* `especializacao_funcionario` (VARCHAR(100))

**Tabela Agendamentos:**

* `id_agendamento` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `id_cliente` (INT, FOREIGN KEY references Clientes(`id_cliente`), NOT NULL)
* `id_veiculo` (INT, FOREIGN KEY references Veículos(`id_veiculo`), NOT NULL)
* `id_servico` (INT, FOREIGN KEY references Serviços(`id_servico`), NOT NULL)
* `id_funcionario` (INT, FOREIGN KEY references Funcionários(`id_funcionario`))
* `data_agendamento` (DATE, NOT NULL)
* `hora_agendamento` (TIME)
* `status_agendamento` (VARCHAR(50))

**Tabela Peças:**

* `id_peca` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `nome_peca` (VARCHAR(100), NOT NULL)
* `descricao_peca` (TEXT)
* `preco_peca` (DECIMAL(10, 2), NOT NULL)
* `estoque_peca` (INT, NOT NULL, DEFAULT 0)

**Tabela OrdensDeServico:**

* `id_ordem_servico` (INT, PRIMARY KEY, AUTO_INCREMENT)
* `id_agendamento` (INT, FOREIGN KEY references Agendamentos(`id_agendamento`), UNIQUE NOT NULL)
* `id_cliente` (INT, FOREIGN KEY references Clientes(`id_cliente`), NOT NULL)
* `id_veiculo` (INT, FOREIGN KEY references Veículos(`id_veiculo`), NOT NULL)
* `id_funcionario` (INT, FOREIGN KEY references Funcionários(`id_funcionario`))
* `data_abertura_os` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
* `data_conclusao_os` (DATE)
* `status_os` (VARCHAR(50))
* `valor_total_os` (DECIMAL(10, 2))

**Tabela OrdemServicoPecas:** (Para relacionamento muitos-para-muitos)

* `id_ordem_servico` (INT, FOREIGN KEY references OrdensDeServico(`id_ordem_servico`), PRIMARY KEY)
* `id_peca` (INT, FOREIGN KEY references Peças(`id_peca`), PRIMARY KEY)
* `quantidade_peca` (INT, NOT NULL, DEFAULT 1)
* `preco_unitario_peca` (DECIMAL(10, 2), NOT NULL)

## Script SQL para Criação do Esquema

```sql
-- Criação da tabela Clientes
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nome_cliente VARCHAR(100) NOT NULL,
    endereco_cliente VARCHAR(255),
    telefone_cliente VARCHAR(20),
    email_cliente VARCHAR(100)
);

-- Criação da tabela Veículos
CREATE TABLE Veículos (
    id_veiculo INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT NOT NULL,
    marca_veiculo VARCHAR(50) NOT NULL,
    modelo_veiculo VARCHAR(50) NOT NULL,
    ano_veiculo INT,
    placa_veiculo VARCHAR(10) UNIQUE NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
);

-- Criação da tabela Serviços
CREATE TABLE Serviços (
    id_servico INT PRIMARY KEY AUTO_INCREMENT,
    nome_servico VARCHAR(100) NOT NULL,
    descricao_servico TEXT,
    preco_servico DECIMAL(10, 2) NOT NULL
);

-- Criação da tabela Funcionários
CREATE TABLE Funcionários (
    id_funcionario INT PRIMARY KEY AUTO_INCREMENT,
    nome_funcionario VARCHAR(100) NOT NULL,
    cargo_funcionario VARCHAR(50),
    especializacao_funcionario VARCHAR(100)
);

-- Criação da tabela Agendamentos
CREATE TABLE Agendamentos (
    id_agendamento INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT NOT NULL,
    id_veiculo INT NOT NULL,
    id_servico INT NOT NULL,
    id_funcionario INT,
    data_agendamento DATE NOT NULL,
    hora_agendamento TIME,
    status_agendamento VARCHAR(50),
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente),
    FOREIGN KEY (id_veiculo) REFERENCES Veículos(id_veiculo),
    FOREIGN KEY (id_servico) REFERENCES Serviços(id_servico),
    FOREIGN KEY (id_funcionario) REFERENCES Funcionários(id_funcionario)
);

-- Criação da tabela Peças
CREATE TABLE Peças (
    id_peca INT PRIMARY KEY AUTO_INCREMENT,
    nome_peca VARCHAR(100) NOT NULL,
    descricao_peca TEXT,
    preco_peca DECIMAL(10, 2) NOT NULL,
    estoque_peca INT NOT NULL DEFAULT 0
);

-- Criação da tabela OrdensDeServico
CREATE TABLE OrdensDeServico (
    id_ordem_servico INT PRIMARY KEY AUTO_INCREMENT,
    id_agendamento INT UNIQUE NOT NULL,
    id_cliente INT NOT NULL,
    id_veiculo INT NOT NULL,
    id_funcionario INT,
    data_abertura_os TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data_conclusao_os DATE,
    status_os VARCHAR(50),
    valor_total_os DECIMAL(10, 2),
    FOREIGN KEY (id_agendamento) REFERENCES Agendamentos(id_agendamento),
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente),
    FOREIGN KEY (id_veiculo) REFERENCES Veículos(id_veiculo),
    FOREIGN KEY (id_funcionario) REFERENCES Funcionários(id_funcionario)
);

-- Criação da tabela OrdemServicoPecas (Relacionamento muitos-para-muitos)
CREATE TABLE OrdemServicoPecas (
    id_ordem_servico INT,
    id_peca INT,
    quantidade_peca INT NOT NULL DEFAULT 1,
    preco_unitario_peca DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (id_ordem_servico, id_peca),
    FOREIGN KEY (id_ordem_servico) REFERENCES OrdensDeServico(id_ordem_servico),
    FOREIGN KEY (id_peca) REFERENCES Peças(id_peca)
);
```

## Persistência de Dados para Testes (Exemplo)

```sql
-- Inserindo clientes
INSERT INTO Clientes (nome_cliente, endereco_cliente, telefone_cliente, email_cliente) VALUES
('João Silva', 'Rua A, 123', '11 99999-8888', 'joao.silva@email.com'),
('Maria Souza', 'Avenida B, 456', '21 88888-7777', 'maria.souza@email.com');

-- Inserindo veículos
INSERT INTO Veículos (id_cliente, marca_veiculo, modelo_veiculo, ano_veiculo, placa_veiculo) VALUES
(1, 'Volkswagen', 'Gol', 2015, 'ABC-1234'),
(1, 'Fiat', 'Uno', 2010, 'DEF-5678'),
(2, 'Chevrolet', 'Onix', 2020, 'GHI-9012');

-- Inserindo serviços
INSERT INTO Serviços (nome_servico, descricao_servico, preco_servico) VALUES
('Revisão Básica', 'Troca de óleo, filtro de óleo e filtro de ar', 150.00),
('Troca de Pneus', 'Substituição de quatro pneus', 400.00),
('Alinhamento e Balanceamento', 'Ajuste de direção e balanceamento das rodas', 120.00);

-- Inserindo funcionários
INSERT INTO Funcionários (nome_funcionario, cargo_funcionario, especializacao_funcionario) VALUES
('Pedro Alves', 'Mecânico', 'Motor e Suspensão'),
('Ana Clara', 'Atendente', 'Recepção e Agendamentos');

-- Inserindo agendamentos
INSERT INTO Agendamentos (id_cliente, id_veiculo, id_servico, id_funcionario, data_agendamento, hora_agendamento, status_agendamento) VALUES
(1, 1, 1, 1, '2025-04-15', '09:00:00', 'Agendado'),
(2, 3, 2, 1, '2025-04-16', '14:00:00', 'Confirmado');

-- Inserindo peças
INSERT INTO Peças (nome_peca, descricao_peca, preco_peca, estoque_peca) VALUES
('Óleo Motor 5W30', 'Óleo lubrificante para motor', 35.00, 50),
('Filtro de Óleo', 'Filtro para óleo do motor', 15.00, 100),
('Pneu Aro 14', 'Pneu para roda aro 14', 100.00, 30);

-- Inserindo ordens de serviço
INSERT INTO OrdensDeServico (id_agendamento, id_cliente, id_veiculo, id_funcionario, data_conclusao_os, status_os, valor_total_os) VALUES
(1, 1, 1, 1, '2025-04-15', 'Concluído', 185.00),
(2, 2, 3, 1, '2025-04-16', 'Em Andamento', null);

-- Inserindo peças utilizadas na ordem de serviço
INSERT INTO OrdemServicoPecas (id_ordem_servico, id_peca, quantidade_peca, preco_unitario_peca) VALUES
(1, 1, 1, 35.00),
(1, 2, 1, 15.00),
(2, 3, 4, 100.00);
```

## Queries SQL Mais Complexas

A seguir, são apresentadas algumas queries SQL mais complexas que demonstram a utilização de diferentes cláusulas para fornecer uma perspectiva mais aprofundada dos dados.

**Pergunta 1: Liste o nome dos clientes e a quantidade de veículos que cada um possui, ordenando pelo nome do cliente.**

```sql
SELECT
    c.nome_cliente,
    COUNT(v.id_veiculo) AS quantidade_veiculos
FROM
    Clientes c
LEFT JOIN
    Veículos v ON c.id_cliente = v.id_cliente
GROUP BY
    c.nome_cliente
ORDER BY
    c.nome_cliente;
```

* **Recuperações simples com SELECT Statement:** `SELECT c.nome_cliente, COUNT(v.id_veiculo)`.
* **Crie expressões para gerar atributos derivados:** `COUNT(v.id_veiculo) AS quantidade_veiculos`.
* **Defina ordenações dos dados com ORDER BY:** `ORDER BY c.nome_cliente`.
* **Crie junções entre tabelas:** `LEFT JOIN Veículos v ON c.id_cliente = v.id_cliente`.

**Pergunta 2: Quais funcionários realizaram agendamentos para clientes no mês de abril de 2025?**

```sql
SELECT DISTINCT
    f.nome_funcionario
FROM
    Funcionários f
JOIN
    Agendamentos a ON f.id_funcionario = a.id_funcionario
WHERE
    STRFTIME('%Y-%m', a.data_agendamento) = '2025-04';
```

* **Recuperações simples com SELECT Statement:** `SELECT DISTINCT f.nome_funcionario`.
* **Filtros com WHERE Statement:** `WHERE STRFTIME('%Y-%m', a.data_agendamento) = '2025-04'`.
* **Crie junções entre tabelas:** `JOIN Agendamentos a ON f.id_funcionario = a.id_funcionario`.

**Pergunta 3: Calcule o valor total de cada ordem de serviço, incluindo o custo dos serviços e das peças utilizadas.**

```sql
SELECT
    os.id_ordem_servico,
    c.nome_cliente,
    v.placa_veiculo,
    s.nome_servico,
    COALESCE(os.valor_total_os, 0) + COALESCE(SUM(osp.quantidade_peca * osp.preco_unitario_peca), 0) AS valor_total_final
FROM
    OrdensDeServico os
JOIN
    Clientes c ON os.id_cliente = c.id_cliente
JOIN
    Veículos v ON os.id_veiculo = v.id_veiculo
JOIN
    Agendamentos a ON os.id_agendamento = a.id_agendamento
JOIN
    Serviços s ON a.id_servico = s.id_servico
LEFT JOIN
    OrdemServicoPecas osp ON os.id_ordem_servico = osp.id_ordem_servico
GROUP BY
    os.id_ordem_servico, c.nome_cliente, v.placa_veiculo, s.nome_servico, os.valor_total_os
ORDER BY
    os.id_ordem_servico;
```

* **Recuperações simples com SELECT Statement:** `SELECT os.id_ordem_servico, c.nome_cliente, v.placa_veiculo, s.nome_servico, ...`.
* **Crie expressões para gerar atributos derivados:** `COALESCE(os.valor_total_os, 0) + COALESCE(SUM(osp.quantidade_peca * osp.preco_unitario_peca), 0) AS valor_total_final`.
* **Crie junções entre tabelas:** Múltiplos `JOIN` e `LEFT JOIN` para combinar informações de diversas tabelas.
* **Agregação com GROUP BY:** `GROUP BY os.id_ordem_servico, c.nome_cliente, v.placa_veiculo, s.nome_servico, os.valor_total_os`.
* **Defina ordenações dos dados com ORDER BY:** `ORDER BY os.id_ordem_servico`.

**Pergunta 4: Liste os nomes dos serviços que tiveram mais de um agendamento no mês de abril de 2025.**

```sql
SELECT
    s.nome_servico,
    COUNT(a.id_agendamento) AS total_agendamentos
FROM
    Serviços s
JOIN
    Agendamentos a ON s.id_servico = a.id_servico
WHERE
    STRFTIME('%Y-%m', a.data_agendamento) = '2025-04'
GROUP BY
    s.nome_servico
HAVING
    COUNT(a.id_agendamento) > 1
ORDER BY
    total_agendamentos DESC;
```

* **Recuperações simples com SELECT Statement:** `SELECT s.nome_servico, COUNT(a.id_agendamento)`.
* **Filtros com WHERE Statement:** `WHERE STRFTIME('%Y-%m', a.data_agendamento) = '2025-04'`.
* **Crie expressões para gerar atributos derivados:** `COUNT(a.id_agendamento) AS total_agendamentos`.
* **Agregação com GROUP BY:** `GROUP BY s.nome_servico`.
* **Condições de filtros aos grupos – HAVING Statement:** `HAVING COUNT(a.id_agendamento) > 1`.
* **Defina ordenações dos dados com ORDER BY:** `ORDER BY total_agendamentos DESC`.

**Pergunta 5: Quais clientes possuem veículos da marca 'Volkswagen' e fizeram algum agendamento?**

```sql
SELECT DISTINCT
    c.nome_cliente
FROM
    Clientes c
JOIN
    Veículos v ON c.id_cliente = v.id_cliente
JOIN
    Agendamentos a ON c.id_cliente = a.id_cliente
WHERE
    v.marca_veiculo = 'Volkswagen';
```

* **Recuperações simples com SELECT Statement:** `SELECT DISTINCT c.nome_cliente`.
* **Filtros com WHERE Statement:** `WHERE v.marca_veiculo = 'Volkswagen'`.
* **Crie junções entre tabelas:** Múltiplos `JOIN` para combinar informações de `Clientes`, `Veículos` e `Agendamentos`.

**Pergunta 6: Liste o nome dos funcionários e a quantidade de ordens de serviço que cada um concluiu (status 'Concluído').**

```sql
SELECT
    f.nome_funcionario,
    COUNT(os.id_ordem_servico) AS total_os_concluidas
FROM
    Funcionários f
LEFT JOIN
    OrdensDeServico os ON f.id_funcionario = os.id_funcionario
WHERE
    os.status_os = 'Concluído'
GROUP BY
    f.nome_funcionario
ORDER BY
    total_os_concluidas DESC;
```

* **Recuperações simples com SELECT Statement:** `SELECT f.nome_funcionario, COUNT(os.id_ordem_servico)`.
* **Filtros com WHERE Statement:** `WHERE os.status_os = 'Concluído'`.
* **Crie expressões para gerar atributos derivados:** `COUNT(os.id_ordem_servico) AS total_os_concluidas`.
* **Agregação com GROUP BY:** `GROUP BY f.nome_funcionario`.
* **Defina ordenações dos dados com ORDER BY:** `ORDER BY total_os_concluidas DESC`.
* **Crie junções entre tabelas:** `LEFT JOIN OrdensDeServico os ON f.id_funcionario = os.id_funcionario`.

## Considerações Finais

Este projeto apresenta o desenvolvimento completo do esquema lógico de um banco de dados para uma oficina, desde a modelagem das tabelas e seus relacionamentos até a implementação do script SQL, a inserção de dados para testes e a elaboração de queries complexas. As queries demonstram a capacidade de recuperar e manipular os dados para responder a perguntas relevantes para a gestão da oficina.
```
