# Projeto Final - Banco de Dados

Projeto final do módulo de Banco de Dados da trilha de Data Science do Programa Santander Coders 2024.2, ministrado pelo instrutor [Lucas Ximenes](https://www.linkedin.com/in/luc4sxf/).

**Grupo** 
Giovanni Ornellas ([GitHub](https://github.com/Giovanni-Ornellas)/[Linkedin](https://www.linkedin.com/in/giovanni-ornellas-419610227/)) 
Lívia Nobre ([GitHub]()/[Linkedin](https://www.linkedin.com/in/livianobremesquita/)) 
Maria Paula Andrade ([GitHub](https://github.com/MariaPaulaAndrade)/[Linkedin](https://www.linkedin.com/in/maria-paula-andrade/)) 
Murilo da Silva ([GitHub](https://github.com/MuriloSBenedito)/[Linkedin](http://www.linkedin.com/in/murilo-silva-bb2741a1))

## Modelagem e normalização de banco de dados relacionais

Certo dia, um dos gestores do banco em que você trabalha como cientista de dados procurou você pedindo ajuda para projetar um pequeno banco de dados com o objetivo de mapear os clientes da companhia pelos diferentes produtos financeiros que eles contrataram.

O gestor explicou que o banco tinha uma grande quantidade de clientes e oferecia uma variedade de produtos financeiros, como cartões de crédito, empréstimos, seguros e investimentos. No entanto, eles estavam tendo dificuldades para entender quais produtos eram mais populares entre os clientes e como esses produtos estavam interagindo entre si.

Como ponto de partida, o gestor deixou claro que um cliente pode contratar vários produtos diferentes ao passo que um mesmo produto pode também estar associado a vários clientes diferentes e elaborou um rústico esboço de banco de dados com duas tabelas, da seguinte forma:

TABELA 1
```sql
Nome da tabela: cliente
Colunas: codigo_cliente, nome_cliente, sobrenome_cliente, telefone_cliente, municipio_cliente,
codigo_tipo_cliente, tipo_cliente
```

TABELA 2
```sql
Nome da tabela: produto
Colunas: codigo_produto, nome_produto, descricao_produto, codigo_tipo_produto, tipo_produto,
codigo_diretor_responsavel, nome_diretor_responsavel, email_diretor_responsavel
```

### Questão 1
**Ainda sem fazer normalizações, apresente o modelo conceitual deste esboço oferecido pelo gestor, destacando atributos chaves e apresentando também a cardinalidade dos relacionamentos.**

### Modelo conceitual do esboço oferecido:

![diagrama_conceitual.drawio](https://hackmd.io/_uploads/SyATIvDAR.png)

Temos duas tabelas: `cliente` e `produto`.

- **Tabela cliente:**
  - **Atributos chave:** `codigo_cliente`
  - Outros atributos: `nome_cliente`, `sobrenome_cliente`, `telefone_cliente`, `municipio_cliente`, `codigo_tipo_cliente`, `tipo_cliente`
  - Cada cliente pode ter vários produtos.

- **Tabela produto:**
  - **Atributos chave:** `codigo_produto`
  - Outros atributos: `nome_produto`, `descricao_produto`, `codigo_tipo_produto`, `tipo_produto`, `codigo_diretor_responsavel`, `nome_diretor_responsavel`, `email_diretor_responsavel`
  - Cada produto pode estar associado a vários clientes.

**Cardinalidade:** Relação muitos-para-muitos entre `cliente` e `produto` (um cliente pode contratar vários produtos e um produto pode ser associado a vários clientes).

---
### Questão 2
**Agora apresente um modelo lógico que expresse as mesmas informações e relacionamentos descritos no modelo original, mas decompondo-os quando necessário para que sejam respeitadas as 3 primeiras formas normais. Destaque atributos chaves e apresente também a cardinalidade dos relacionamentos.**

**Definições das Formas Normais**

1. **Primeira Forma Normal (1FN)**
   - **Definição:** Uma tabela (entidade) está na 1ª forma normal quando todos os seus atributos possuem valores indivisíveis (atômicos) e cada linha representa um registro único. Cada registro único é garantido por uma chave primária, que é um atributo ou uma combinação de atributos que identifica de forma exclusiva cada linha da tabela.
   - **Exemplo:** Considere uma tabela de "Estudantes" que armazena informações sobre os alunos. Se tivermos uma coluna chamada "telefones" que contém múltiplos números separados por vírgulas, essa tabela não estaria na 1FN. Para atender a 1FN, devemos dividir essa coluna em registros separados ou criar uma tabela associativa para os números de telefone.

2. **Segunda Forma Normal (2FN)**
   - **Definição:** Uma tabela (entidade) está na 2ª Forma Normal quando todos os seus atributos não-chave dependem unicamente da chave primária. Isso significa que não deve haver dependências parciais, onde um atributo depende apenas de parte da chave primária em vez de depender de toda a chave.
   - **Exemplo:** Se tivermos uma tabela "Vendas" que contém as colunas "id_venda", "id_produto", "nome_produto" e "quantidade", e "id_venda" e "id_produto" formam a chave primária, o "nome_produto" depende apenas de "id_produto". Para atender à 2FN, devemos criar uma tabela separada para "Produtos", que contém "id_produto" e "nome_produto", eliminando a dependência parcial.

3. **Terceira Forma Normal (3FN)**
   - **Definição:** Uma tabela (entidade) está na 3ª Forma Normal quando não há dependências transitivas entre os atributos. Isso significa que um atributo não-chave não deve depender de outro atributo não-chave.
   - **Exemplo:** Considere uma tabela "Funcionários" com as colunas "id_funcionario", "nome_funcionario", "id_departamento", e "nome_departamento". Se "nome_departamento" depende de "id_departamento", então temos uma dependência transitiva, já que "id_departamento" não é uma chave primária. Para atender à 3FN, devemos separar "Departamentos" em uma tabela distinta, contendo "id_departamento" e "nome_departamento", garantindo que todos os atributos não-chave dependam diretamente da chave primária.

**Modelo lógico com normalização (até 3FN)**
![modelo_logico.drawio](https://hackmd.io/_uploads/H1pQPvP0R.png)

1. **Tabela cliente:**
   - `codigo_cliente` (PK)
   - `nome_cliente`
   - `sobrenome_cliente`
   - `telefone_cliente`
   - `municipio_cliente`
   - `codigo_tipo_cliente` (FK para a tabela `tipo_cliente`)

2. **Tabela tipo_cliente:**
   - `codigo_tipo_cliente` (PK)
   - `tipo_cliente`

3. **Tabela produto:**
   - `codigo_produto` (PK)
   - `nome_produto`
   - `descricao_produto`
   - `codigo_tipo_produto` (FK para a tabela `tipo_produto`)
   - `codigo_diretor_responsavel` (FK para a tabela `diretor`)

4. **Tabela tipo_produto:**
   - `codigo_tipo_produto` (PK)
   - `tipo_produto`

5. **Tabela diretor:**
   - `codigo_diretor_responsavel` (PK)
   - `nome_diretor_responsavel`
   - `email_diretor_responsavel`

6. **Tabela contrato (associação muitos-para-muitos):**
   - `codigo_contrato` (PK)
   - `codigo_cliente` (FK para `cliente`)
   - `codigo_produto` (FK para `produto`)
   - 'valor_contrato'
   
---

## Consultas SQL simples e complexas em um banco de dados relacional
Um exemplo de modelo de banco de dados com relacionamento muitos-para-muitos pode ser o de um e-commerce que tem produtos e categorias, onde um produto pode pertencer a várias categorias e uma categoria pode estar associada a vários produtos. Nesse caso, teríamos duas tabelas: "produtos" e "categorias", com uma tabela intermediária "produtos_categorias" para relacionar os produtos às suas categorias.

```sql
CREATE TABLE produtos (
id INT PRIMARY KEY AUTO_INCREMENT,
nome VARCHAR(100) NOT NULL,
preco DECIMAL(10, 2) NOT NULL
);
```

```sql
CREATE TABLE categorias (
id INT PRIMARY KEY AUTO_INCREMENT,
nome VARCHAR(100) NOT NULL
);
```

```sql
CREATE TABLE produtos_categorias (
produto_id INTEGER REFERENCES produtos(id),
categoria_id INTEGER REFERENCES categorias(id)
);
```

### Questão 3
**Liste os nomes de todos os produtos que custam mais de 100 reais, ordenando-os primeiramente pelo preço e em segundo lugar pelo nome. Use alias para mostrar o nome da coluna nome como "Produto" e da coluna preco como "Valor". A resposta da consulta não deve mostrar outras colunas de dados.**

- **Listando produtos que custam mais de 100 reais:**

```sql
SELECT nome AS "Produto", preco AS "Valor"
FROM produtos
WHERE preco > 100
ORDER BY preco ASC, nome ASC;
```

### Questão 4
**Liste todos os ids e preços de produtos cujo preço seja maior do que a média de todos os preços encontrados na tabela "produtos".**

- **Listando produtos com preço maior que a média:**

```sql
SELECT id, preco
FROM produtos
WHERE preco > (SELECT AVG(preco) FROM produtos);
```

### Questão 5
**Para cada categoria, mostre o preço médio do conjunto de produtos a ela associados. Caso uma categoria não tenha nenhum produto a ela associada, esta categoria não deve aparecer no resultado final. A consulta deve estar ordenada pelos nomes das categorias.**

- **Preço médio dos produtos por categoria:**

```sql
SELECT categorias.nome, AVG(produtos.preco) AS preco_medio
FROM produtos
JOIN produtos_categorias ON produtos.id = produtos_categorias.produto_id
JOIN categorias ON categorias.id = produtos_categorias.categoria_id
GROUP BY categorias.nome
HAVING COUNT(produtos.id) > 0
ORDER BY categorias.nome ASC;
```

---

## Inserções, alterações e remoções de objetos e dados em um banco de dados relacional

Você está participando de um processo seletivo para trabalhar como cientista de dados na Ada, uma das maiores formadoras do país em áreas correlatadas à tecnologia. Dividido em algumas etapas, o processo tem o objetivo de avaliar você nos quesitos Python, Machine Learning e Bancos de Dados. Ainda que os dois primeiros sejam o cerne da sua atuação no dia a dia, considera-se que Bancos de Dados também constituem um requisito importante e, por isso, esta etapa pode ser a oportunidade que você precisava para se destacar dentre os seus concorrentes, demonstrando um conhecimento mais amplo do que os demais.

### Questão 6
**Com o objetivo de demonstrar o seu conhecimento através de um exemplo contextualizado com o dia-a-dia da escola, utilize os comandos do subgrupo de funções DDL para construir o banco de dados simples abaixo, que representa um relacionamento do tipo 1,n entre as entidades "aluno" e "turma":**


TABELA 1
```sql
Nome da tabela: aluno
Colunas da tabela: id_aluno (INT), nome_aluno (VARCHAR), aluno_alocado (BOOLEAN), id_turma (INT)
```
TABELA 2
```sql
Nome da tabela: turma
Colunas da tabela: id_turma (INT), código_turma (VARCHAR), nome_turma (VARCHAR)
```

**Criação de tabelas (DDL):**

```sql
CREATE TABLE aluno (
  id_aluno INT PRIMARY KEY,
  nome_aluno VARCHAR(100),
  aluno_alocado BOOLEAN,
  id_turma INT
);

CREATE TABLE turma (
  id_turma INT PRIMARY KEY,
  codigo_turma VARCHAR(100),
  nome_turma VARCHAR(100)
);
```

### Questão 7
**Agora que você demonstrou que consegue ser mais do que um simples usuário do banco de dados, mostre separadamente cada um dos códigos DML necessários para cumprir cada uma das etapas a seguir:**

**Inserções e atualizações (DML):**

- **a) Inserir pelo menos duas turmas diferentes na tabela de turma**

```sql
INSERT INTO turma (id_turma, codigo_turma, nome_turma)
VALUES (1, 'TURMA01', 'Geografia'), (2, 'TURMA02', 'História');
```

- **b) Inserir pelo menos 1 aluno alocado em cada uma destas turmas na tabela aluno (todos com NULL na coluna aluno_alocado);**

```sql
INSERT INTO aluno (id_aluno, nome_aluno, aluno_alocado, id_turma)
VALUES (1, 'Giovanni Ornellas', NULL, 1), (2, 'Maria Paula', NULL, 2);
```

- **c) Inserir pelo menos 2 alunos não alocados em nenhuma turma na tabela aluno (todos com NULL na coluna aluno_alocado)**

```sql
INSERT INTO aluno (id_aluno, nome_aluno, aluno_alocado, id_turma)
VALUES (3, 'Murilo da Silva', NULL, NULL), (4, 'Lívia Nobre', NULL, NULL);
```

- **d) Atualizar a coluna aluno_alocado da tabela aluno, de modo que os alunos associados a uma disciplina recebam o valor `True` e alunos não associdos a nenhuma disciplina recebam o falor `False` para esta coluna.**

```sql
UPDATE aluno
SET aluno_alocado = TRUE
WHERE id_turma IS NOT NULL;

UPDATE aluno
SET aluno_alocado = FALSE
WHERE id_turma IS NULL;
```
