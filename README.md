## Construindo seu Primeiro Projeto Lógico de Banco de Dados.

![Screenshot_20250614-182332](https://github.com/user-attachments/assets/d886965b-ba46-4652-82f1-f9470e7f1e96)

## Bootcamp, Heineken - Inteligência Artificial Aplicada a Dados com Copilot. Pareceria Heineken e DIO.



# 🛒 Primeiro Projeto Lógico de Banco de Dados — E-commerce

![Banner do Projeto](https://github.com/user-attachments/assets/d886965b-ba46-4652-82f1-f9470e7f1e96)

> **"Um modelo bem feito não é apenas técnico — é a tradução de um problema de negócio em estrutura que pode ser consultada, auditada e evoluída."**  
> — Adaptado de Meigarom Lopes

<div align="center">

[![Portfólio](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
[![Bootcamp](https://img.shields.io/badge/Bootcamp-Heineken_×_DIO-00A86B?style=for-the-badge&logo=databricks&logoColor=white)](https://www.dio.me)
[![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com)

</div>

---

## 📋 Índice

- [1. Problema de Negócio](#1-problema-de-negócio)
- [2. Contexto](#2-contexto)
- [3. Premissas da Análise](#3-premissas-da-análise)
- [4. Estratégia da Solução](#4-estratégia-da-solução)
- [5. Estrutura do Projeto](#5-estrutura-do-projeto)
- [6. Modelo de Entidade-Relacionamento](#6-modelo-de-entidade-relacionamento)
- [7. Esquema Lógico Relacional](#7-esquema-lógico-relacional)
- [8. Queries SQL Complexas](#8-queries-sql-complexas)
- [9. Insights Técnicos](#9-insights-técnicos)
- [10. Resultados](#10-resultados)
- [11. Aprendizados](#11-aprendizados)
- [12. Próximos Passos](#12-próximos-passos)
- [13. Como Executar](#13-como-executar)
- [14. Tecnologias Utilizadas](#14-tecnologias-utilizadas)
- [15. Contato](#15-contato)

---

## 1. Problema de Negócio

Plataformas de e-commerce precisam gerenciar simultaneamente múltiplas dimensões de dados: clientes de natureza jurídica diferente (PF e PJ), múltiplas formas de pagamento por pedido, rastreabilidade de entregas, controle de estoque por lote e promoções com vigência definida. Sem um modelo relacional bem estruturado, essas informações ficam dispersas, inconsistentes e impossíveis de consultar de forma analítica.

O problema central é: **como estruturar um banco de dados relacional para e-commerce que reflita fielmente as regras de negócio — incluindo especializações disjuntas de cliente, múltiplos pagamentos e rastreio de entrega — e que permita responder perguntas operacionais e estratégicas por meio de SQL?**

---

## 2. Contexto

Este projeto foi desenvolvido no **Bootcamp Heineken — Inteligência Artificial Aplicada a Dados com Copilot**, em parceria com a DIO. O desafio propõe replicar e refinar a modelagem lógica de um banco de dados para um cenário de e-commerce, partindo de um modelo EER (Enhanced Entity-Relationship) e evoluindo até a implementação completa com queries analíticas.

Três refinamentos específicos foram exigidos em relação ao modelo base:

- **Cliente PJ e PF**: uma conta pode ser Pessoa Física ou Jurídica, mas nunca as duas — especialização disjunta implementada via tabelas `clientePF` e `clientePJ`.
- **Pagamento múltiplo**: um pedido pode ter mais de uma forma de pagamento registrada.
- **Entrega rastreável**: cada pedido possui uma entrega com `statusEntrega` e `codigoRastreio` único.

O cenário é realista o suficiente para simular os principais desafios de modelagem de uma operação de e-commerce de médio porte.

---

## 3. Premissas da Análise

Para a modelagem e implementação, foram adotadas as seguintes premissas:

- A especialização de cliente é **disjunta e total**: todo cliente é obrigatoriamente PF ou PJ, nunca ambos. A integridade é garantida por chave estrangeira e pela lógica de inserção.
- O campo `statusPedido` (ENUM: `pendente`, `enviado`, `cancelado`, `entregue`) é a fonte oficial do ciclo de vida de um pedido.
- O `codigoRastreio` da tabela `entrega` é definido como `UNIQUE` — cada entrega possui um código irrepetível.
- A combinação `idProduto + numeroLote` na tabela `estoque` é única — o mesmo produto pode ter múltiplos lotes, mas não o mesmo lote duas vezes.
- O `precoVenda` em `itemPedido` é registrado no momento da compra, preservando o histórico de preço independentemente de alterações futuras no cadastro do produto.
- A tabela `pagamento` possui relacionamento 1:N com `pedido` para suportar múltiplas formas de pagamento por pedido.

---

## 4. Estratégia da Solução

A solução foi construída em quatro etapas sequenciais:

**Etapa 1 — Análise do modelo EER**  
Leitura e interpretação do diagrama conceitual, mapeando entidades, atributos, cardinalidades e especializações.

**Etapa 2 — Tradução para o modelo lógico relacional**  
Cada entidade virou tabela. Relacionamentos N:M foram resolvidos com tabelas de associação (`produtoCategoria`, `produtoPromocao`, `itemPedido`, `clienteEndereco`). A especialização disjunta de cliente foi implementada com tabelas filhas `clientePF` e `clientePJ`, cada uma compartilhando o `idCliente` como PK e FK.

**Etapa 3 — Documentação do esquema**  
Especificação completa de todas as tabelas: atributos, tipos, constraints, chaves primárias, chaves estrangeiras e relacionamentos.

**Etapa 4 — Desenvolvimento de queries SQL analíticas**  
Elaboração de 18 consultas que cobrem os 9 requisitos do desafio, cada uma respondendo uma pergunta de negócio concreta: desde recuperações simples até sub-queries, JOINs multi-tabela e verificações de integridade da especialização disjunta.

---

## 5. Estrutura do Projeto

```
firstLogicoBD/
│
├── entidadeRelacionamento.md    # Modelo EER, esquema lógico e definição das tabelas
└── perguntasSQLecommerce.md     # 18 queries SQL com casos de uso e scripts
```

---

## 6. Modelo de Entidade-Relacionamento

![Diagrama EER do E-commerce](https://github.com/user-attachments/assets/7b33380a-8839-4331-9a0d-793af4e89a0b)

---

## 7. Esquema Lógico Relacional

O modelo é composto por **15 tabelas**: entidades principais, tabelas de especialização, tabelas de associação e entidades de suporte.

### Entidades principais

| Tabela | PK | Atributos-chave |
|---|---|---|
| `fornecedor` | `idFornecedor` | nome, email, telefone, dataFor |
| `produto` | `idProduto` | nome, preco, statusProduto (ENUM), `idFornecedor` (FK) |
| `estoque` | `idEstoque` | quantidade, localizacao, dataValidade, numeroLote, `idProduto` (FK) |
| `cliente` | `idCliente` | nome, email, telefone, dataCli |
| `endereco` | `idEndereco` | logradouro, cidade, estado, CEP |
| `categoria` | `idCategoria` | nomeCategoria, descricaoCategoria |
| `promocao` | `idPromocao` | nomePromocao, dataInicio, dataFim, valorDesconto |
| `pedido` | `idPedido` | dataPed, statusPedido (ENUM), valorFrete, `idCliente` (FK) |
| `entrega` | `idEntrega` | statusEntrega (ENUM), codigoRastreio (UNIQUE), `idPedido` (FK) |
| `pagamento` | `idPagamento` | tipoPagamento, detalhes, dataPag, `idPedido` (FK) |

### Especialização disjunta de Cliente

| Tabela | PK/FK | Atributo específico |
|---|---|---|
| `clientePF` | `idCliente` (FK = PK) | CPF |
| `clientePJ` | `idCliente` (FK = PK) | CNPJ, razaoSocial |

> Um cliente jamais aparece em ambas as tabelas simultaneamente. A disjunção é garantida por constraint e lógica de aplicação.

### Tabelas de associação (N:M)

| Tabela | Relacionamento | Atributo extra |
|---|---|---|
| `clienteEndereco` | cliente ↔ endereco | tipoEndereco (ENUM) |
| `produtoCategoria` | produto ↔ categoria | — |
| `produtoPromocao` | produto ↔ promocao | — |
| `itemPedido` | pedido ↔ produto | quantidade, precoVenda, `idEstoque` (FK) |

### Diagrama de relacionamentos

```
fornecedor (1) ──── (N) produto (N) ──┬── (N) produtoCategoria ── (N) categoria
                         │             └── (N) produtoPromocao  ── (N) promocao
                         │
                    estoque (N:1)
                         │
cliente (1) ──── (N) pedido (1) ──┬── (N) itemPedido ── produto
     │                             ├── (1) entrega
     ├── clientePF                 └── (N) pagamento
     ├── clientePJ
     └── (N) clienteEndereco ── endereco
```

---

## 8. Queries SQL Complexas

### 8.1 SELECT — Recuperações simples

```sql
-- Pergunta: Quais são todos os fornecedores cadastrados?
SELECT idFornecedor, nome, email, telefone, dataFor
FROM fornecedor;

-- Pergunta: Liste todos os produtos (id e nome).
SELECT idProduto, nome
FROM produto;
```

### 8.2 WHERE — Filtros

```sql
-- Pergunta: Quais produtos têm preço maior que R$ 100,00?
SELECT idProduto, nome, preco
FROM produto
WHERE preco > 100.00;

-- Pergunta: Quais clientes foram cadastrados após 1º de janeiro de 2025?
SELECT idCliente, nome, email, dataCli
FROM cliente
WHERE dataCli > '2025-01-01';
```

### 8.3 Atributos Derivados — Expressões calculadas

```sql
-- Pergunta: Qual seria o preço dobrado de cada produto?
SELECT idProduto,
       nome,
       preco,
       preco * 2 AS preco_dobrado
FROM produto;

-- Pergunta: Qual o valor total estimado de cada pedido (frete + itens)?
SELECT p.idPedido,
       p.valorFrete,
       COALESCE(SUM(ip.quantidade * ip.precoVenda), 0)                AS total_itens,
       p.valorFrete + COALESCE(SUM(ip.quantidade * ip.precoVenda), 0) AS valor_estimado
FROM pedido p
LEFT JOIN itemPedido ip ON ip.idPedido = p.idPedido
GROUP BY p.idPedido, p.valorFrete;
```

### 8.4 ORDER BY — Ordenações

```sql
-- Pergunta: Quais são os 10 produtos mais caros?
SELECT idProduto, nome, preco
FROM produto
ORDER BY preco DESC
LIMIT 10;

-- Pergunta: Liste os clientes em ordem alfabética.
SELECT idCliente, nome
FROM cliente
ORDER BY nome ASC;
```

### 8.5 GROUP BY — Agrupamentos

```sql
-- Pergunta: Quantos produtos cada fornecedor possui cadastrado?
SELECT f.idFornecedor,
       f.nome,
       COUNT(p.idProduto) AS total_produtos
FROM fornecedor f
LEFT JOIN produto p ON p.idFornecedor = f.idFornecedor
GROUP BY f.idFornecedor, f.nome;

-- Pergunta: Qual o total de unidades vendidas por produto em todos os pedidos?
SELECT p.idProduto,
       p.nome,
       SUM(ip.quantidade) AS total_vendido
FROM produto p
JOIN itemPedido ip ON ip.idProduto = p.idProduto
GROUP BY p.idProduto, p.nome;
```

### 8.6 HAVING — Filtros em grupos

```sql
-- Pergunta: Quais fornecedores têm mais de 50 produtos cadastrados?
SELECT f.idFornecedor,
       f.nome,
       COUNT(p.idProduto) AS total_produtos
FROM fornecedor f
JOIN produto p ON p.idFornecedor = f.idFornecedor
GROUP BY f.idFornecedor, f.nome
HAVING COUNT(p.idProduto) > 50;

-- Pergunta: Quais produtos já venderam ao menos 100 unidades?
SELECT p.idProduto,
       p.nome,
       SUM(ip.quantidade) AS total_vendido
FROM produto p
JOIN itemPedido ip ON ip.idProduto = p.idProduto
GROUP BY p.idProduto, p.nome
HAVING SUM(ip.quantidade) >= 100;
```

### 8.7 JOINs — Perspectiva multi-tabela

```sql
-- Pergunta: Para cada pedido, qual o nome do cliente, data e status?
SELECT p.idPedido,
       c.nome         AS cliente,
       p.dataPed,
       p.statusPedido
FROM pedido p
JOIN cliente c ON c.idCliente = p.idCliente;

-- Pergunta: Quais produtos, quantidades e preços fazem parte do pedido 123?
SELECT ip.idPedido,
       pr.nome        AS produto,
       ip.quantidade,
       ip.precoVenda
FROM itemPedido ip
JOIN produto pr ON pr.idProduto = ip.idProduto
WHERE ip.idPedido = 123;

-- Pergunta: Quais pedidos possuem dados de entrega e qual o status de rastreio?
SELECT p.idPedido,
       p.dataPed,
       e.statusEntrega,
       e.codigoRastreio
FROM pedido p
LEFT JOIN entrega e ON e.idPedido = p.idPedido;
```

### 8.8 Sub-queries

```sql
-- Pergunta: Quais clientes ainda não realizaram nenhum pedido?
SELECT idCliente, nome
FROM cliente
WHERE idCliente NOT IN (
    SELECT DISTINCT idCliente
    FROM pedido
);

-- Pergunta: Quais produtos possuem estoque em lote vencido?
SELECT DISTINCT p.idProduto,
                p.nome
FROM produto p
JOIN estoque e ON e.idProduto = p.idProduto
WHERE e.dataValidade < CURDATE();
```

### 8.9 Especialização Disjunta — Integridade PF vs PJ

```sql
-- Pergunta: Liste todos os clientes Pessoa Física com CPF.
SELECT c.idCliente, c.nome, pf.CPF
FROM cliente c
JOIN clientePF pf ON pf.idCliente = c.idCliente;

-- Pergunta: Liste todos os clientes Pessoa Jurídica com CNPJ e razão social.
SELECT c.idCliente, c.nome, pj.CNPJ, pj.razaoSocial
FROM cliente c
JOIN clientePJ pj ON pj.idCliente = c.idCliente;

-- Pergunta: Algum cliente está cadastrado como PF e PJ ao mesmo tempo? (não deveria ocorrer)
SELECT c.idCliente, c.nome
FROM cliente c
JOIN clientePF pf ON pf.idCliente = c.idCliente
JOIN clientePJ pj ON pj.idCliente = c.idCliente;
```

> Se esta query retornar qualquer linha, há uma violação de integridade no modelo disjunto.

---

## 9. Insights Técnicos

**Decisão: especialização disjunta via tabelas filhas com PK = FK**  
Em vez de usar um campo `tipoPessoa` na tabela `cliente` para distinguir PF de PJ, optei por tabelas separadas `clientePF` e `clientePJ`, onde a PK é a própria FK para `cliente`. Isso garante que o banco de dados reflete a regra de negócio estruturalmente, e não apenas por convenção. A query 9.3 funciona como uma verificação de integridade automatizável.

**Decisão: `precoVenda` como campo em `itemPedido`, não herdado de `produto`**  
O preço de um produto pode mudar ao longo do tempo. Registrar `precoVenda` no momento da compra dentro de `itemPedido` preserva o histórico financeiro real de cada pedido, independentemente de reajustes futuros no cadastro. É uma decisão de auditabilidade, não de redundância.

**Decisão: LEFT JOIN na query de valor estimado por pedido**  
Usei `LEFT JOIN` entre `pedido` e `itemPedido` para garantir que pedidos recém-criados — ainda sem itens — apareçam no resultado com `total_itens = 0` via `COALESCE`. Um `INNER JOIN` excluiria silenciosamente esses registros, gerando relatórios incompletos.

**Trade-off: pagamento modelado como 1:N com pedido**  
O desafio exige suporte a múltiplas formas de pagamento por pedido. Modelar `pagamento` com `idPedido` como FK (e não como 1:1) atende esse requisito sem precisar de uma tabela de associação adicional, mantendo a estrutura simples e consultável.

---

## 10. Resultados

Com o modelo implementado, a plataforma de e-commerce passa a ter capacidade de responder perguntas reais de negócio:

- **Rastreabilidade de pedidos**: status, itens, preços históricos, dados de entrega e formas de pagamento — tudo em uma única query com JOIN.
- **Gestão de estoque**: identificação de lotes vencidos antes do despacho, com granularidade por produto e data.
- **Segmentação de clientes**: distinção estrutural entre PF e PJ, com verificação de integridade automatizável via SQL.
- **Análise de fornecedores**: volume de produtos por fornecedor, identificando concentração de catálogo.
- **Controle promocional**: produtos vinculados a promoções com vigência definida, consultáveis por período.

O projeto demonstra que um modelo relacional bem fundamentado transforma requisitos de negócio em dados consultáveis, auditáveis e evolutivos.

---

## 11. Aprendizados

O aspecto mais desafiador foi implementar a **especialização disjunta** de forma que o banco de dados garantisse a regra estruturalmente, e não apenas pela lógica da aplicação. Entender que usar PK = FK nas tabelas filhas é o mecanismo correto — e não um campo `tipo` na tabela pai — foi o principal aprendizado de modelagem deste projeto.

Outro ponto relevante foi perceber que `COALESCE` não é apenas um detalhe cosmético: sem ele, pedidos sem itens desaparecem dos resultados de cálculo de valor, gerando relatórios financeiros incorretos silenciosamente.

O que faria diferente hoje: adicionaria um campo `dataAlteracao` nas tabelas de status (`pedido`, `entrega`) para rastrear o histórico de mudanças de estado — informação crítica em operações logísticas reais.

---

## 12. Próximos Passos

- Implementar o DDL completo com `CREATE TABLE`, constraints e índices
- Adicionar dados de teste (DML) para todas as tabelas, viabilizando execução real das queries
- Criar `VIEWs` analíticas: ranking de produtos mais vendidos, clientes inativos, estoque crítico
- Desenvolver `STORED PROCEDURES` para abertura de pedido com validação de estoque
- Implementar `TRIGGER` para atualizar `statusEntrega` automaticamente ao concluir o pedido
- Containerizar o ambiente com Docker + MySQL para setup reproduzível

---

## 13. Como Executar

**Pré-requisitos**

| Ferramenta | Versão mínima |
|---|---|
| MySQL Server | 8.0+ |
| MySQL Workbench (opcional) | 8.0+ |
| Git | qualquer |

**Passos**

```bash
# 1. Clone o repositório
git clone https://github.com/Santosdevbjj/firstLogicoBD.git
cd firstLogicoBD
```

> Abra `entidadeRelacionamento.md` para revisar o modelo lógico completo.  
> Execute as queries de `perguntasSQLecommerce.md` diretamente no MySQL Workbench ou via CLI, após criar e popular o banco de dados com o DDL/DML correspondente.

---

## 14. Tecnologias Utilizadas

| Tecnologia | Finalidade |
|---|---|
| MySQL 8.0 | SGBD relacional — modelagem e consultas |
| SQL (ANSI) | DDL, DML e queries analíticas |
| Modelo EER | Esquema conceitual de origem |
| Modelo Relacional | Esquema lógico implementado |
| GitHub | Versionamento e portfólio público |

---

## 15. Contato

<div align="center">

[![Portfólio](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)

</div>

---









---

**Contato:**

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://santosdevbjj.github.io/portfolio/)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz) 

---



