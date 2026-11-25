# db_loja_tech

# 🛒 TechStore Database — Consultas SQL, Stored Procedures e Views

![MySQL](https://img.shields.io/badge/MySQL-8.0+-005C84?style=for-the-badge&logo=mysql&logoColor=white)
![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen?style=for-the-badge)

Repositório desenvolvido para a atividade prática da disciplina de **Banco de Dados**, utilizando o ecossistema da loja fictícia **TechStore** (`loja_tech`). O projeto abrange consultas SQL fundamentais, construção de *Stored Procedures* seguindo boas práticas de engenharia de dados e um estudo comparativo sobre o uso de *Views*.

---

## 🗂️ Estrutura do Repositório

```text
📁 techstore-sql-procedures/
 ├── 📄 schema_loja_tech.sql       # Script de criação do banco, tabelas e carga inicial
 ├── 📄 resolucao_atividade.sql    # Resolução das 10 queries + 5 Stored Procedures
 └── 📄 README.md                  # Documentação completa, pesquisa sobre Views e referências
```

---

## 🗄️ Modelo Entidade-Relacionamento (`loja_tech`)

O banco de dados é composto por 6 tabelas principais:
* `categorias`: Agrupamento dos produtos (Notebooks, Smartphones, Periféricos, etc.).
* `produtos`: Catálogo com controle de preço, estoque atual e estoque mínimo.
* `clientes`: Cadastro de clientes com validação de CPF e e-mail únicos.
* `pedidos`: Cabeçalho dos pedidos com `ENUM` de status e valor total.
* `itens_pedido`: Detalhamento dos itens com coluna calculada (`GENERATED ALWAYS AS`) para o subtotal.
* `log_operacoes`: Tabela de auditoria para rastreamento de operações.

---

## 🚀 Como Executar o Projeto

1. Abra o **MySQL Workbench** (ou seu cliente MySQL de preferência).
2. Execute primeiro o arquivo `schema_loja_tech.sql` para criar o banco de dados `loja_tech` e popular os registros iniciais.
3. Em seguida, abra e execute o arquivo `resolucao_atividade.sql` para rodar as consultas e criar as *Stored Procedures*.
4. Teste os procedimentos utilizando os comandos `CALL` comentados ao final de cada exercício.

---

## 🛠️ Resumo das Implementações

### Parte 1: Consultas SQL (10 Desafios)
* Projeção simples (`SELECT`) e filtragem relacional (`WHERE`).
* Ordenação decrescente (`ORDER BY ... DESC`) e busca textual parcial (`LIKE '%Monitor%'`).
* Funções de agregação (`COUNT`, `SUM`) com uso de *aliases* (`AS`).
* Relacionamento entre tabelas utilizando `INNER JOIN`.

### Parte 2: Stored Procedures (5 Exercícios)
Todas as *procedures* foram desenvolvidas utilizando `DROP PROCEDURE IF EXISTS`, controle de `DELIMITER //` e prefixo `p_` nos parâmetros:

| Procedure | Parâmetros | Objetivo |
| :--- | :--- | :--- |
| `listar_categorias_ativas` | *Nenhum* | Lista categorias com `ativo = TRUE` em ordem alfabética. |
| `buscar_pedidos_por_status` | `IN p_status` | Filtra pedidos de acordo com o status informado. |
| `inserir_categoria` | `IN p_nome, IN p_descricao` | Insere uma nova categoria no catálogo. |
| `atualizar_telefone_cliente` | `IN p_id_cliente, IN p_telefone` | Atualiza o telefone de um cliente específico. |
| `contar_produtos_categoria` | `IN p_id_categoria, OUT p_total` | Retorna a quantidade de produtos de uma categoria via variável de saída. |

---

## 🔍 Pesquisa: 5 Situações para Usar `VIEW` em vez de `STORED PROCEDURE`

Embora ambas fiquem armazenadas no servidor de banco de dados, **Views** e **Stored Procedures** possuem propósitos distintos:

1. **Composição dentro de outras consultas SQL:** Uma *View* funciona como uma tabela virtual, permitindo aplicar novos filtros (`WHERE`), ordenações (`ORDER BY`) ou novos `JOIN`s diretamente sobre ela — algo impossível com o retorno de uma *Stored Procedure*.
2. **Controle de acesso granular e LGPD (Segurança de Colunas):** Permite expor apenas colunas não sensíveis de uma tabela (ocultando CPF e data de nascimento dos clientes, por exemplo) para equipes de atendimento sem precisar engessar as consultas em procedimentos fixos.
3. **Integração nativa com ferramentas de BI:** Plataformas como Power BI, Metabase e Looker reconhecem *Views* como fontes de dados padrão, facilitando a criação dinâmica de gráficos e filtros sem exigir chamadas procedurais (`CALL`).
4. **Abstração de `JOIN`s complexos e repetitivos:** Centraliza relacionamentos frequentes (como unir `itens_pedido`, `produtos` e `categorias`) em uma visão única, reduzindo erros de escrita de código no dia a dia.
5. **Mapeamento direto com ORMs:** Frameworks como Prisma, Hibernate e Entity Framework mapeiam *Views* como entidades somente-leitura, preservando recursos nativos como paginação e filtros dinâmicos na aplicação.

---

## 📚 Referências Bibliográficas

* **Material de Aula:** *Stored Procedures no MySQL — Banco de Dados da Aula: `loja_tech`* (Apostila da disciplina).
* **ORACLE / MYSQL.** *MySQL 8.0 Reference Manual: Stored Objects (Stored Procedures and Views)*. Disponível em: [dev.mysql.com/doc/refman/8.0/en/stored-objects.html](https://dev.mysql.com/doc/refman/8.0/en/stored-objects.html).
* **SILBERSCHATZ, A.; KORTH, H. F.; SUDARSHAN, S.** *Sistema de Banco de Dados*. 7ª ed. Rio de Janeiro: LTC, 2020.
* **ELMASRI, R.; NAVATHE, S. B.** *Sistemas de Banco de Dados*. 7ª ed. São Paulo: Pearson, 2019.
