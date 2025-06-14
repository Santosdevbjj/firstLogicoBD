## Aqui está uma coletânea de consultas SQL, do mais simples ao mais complexo, cobrindo todos os requisitos solicitados.

 ## Para cada consulta, apresento:

## 1. A pergunta (o “caso de uso”) que ela resolve.


## 2. O script SQL correspondente.


**1. Recuperações simples com SELECT**

**Pergunta 1.1: Quais são todos os fornecedores cadastrados?**

SELECT idFornecedor, nome, email, telefone, dataFor
FROM fornecedor;

**Pergunta 1.2: Liste todos os produtos (apenas id e nome).**

SELECT idProduto, nome
FROM produto;



**2. Filtros com WHERE**

**Pergunta 2.1: Quais produtos têm preço maior que R$ 100,00?**

SELECT idProduto, nome, preco
FROM produto
WHERE preco > 100.00;

**Pergunta 2.2: Quais clientes foram cadastrados após 1º de janeiro de 2025?**

SELECT idCliente, nome, email, dataCli
FROM cliente
WHERE dataCli > '2025-01-01';




**3. Atributos derivados (expressões)**

**Pergunta 3.1: Mostrar produtos com o dobro do preço atual (campo calculado).**

SELECT idProduto,
       nome,
       preco,
       preco * 2 AS preco_dobrado
FROM produto;

**Pergunta 3.2: Para cada pedido, calcular o valor total estimado (valorFrete + soma dos itens).**

SELECT p.idPedido,
       p.valorFrete,
       COALESCE(SUM(ip.quantidade * ip.precoVenda), 0) AS total_itens,
       p.valorFrete + COALESCE(SUM(ip.quantidade * ip.precoVenda), 0) AS valor_estimado
FROM pedido p
LEFT JOIN itemPedido ip ON ip.idPedido = p.idPedido
GROUP BY p.idPedido, p.valorFrete;




**4. Ordenação com ORDER BY**

**Pergunta 4.1: Quais são os 10 produtos mais caros?**

SELECT idProduto, nome, preco
FROM produto
ORDER BY preco DESC
LIMIT 10;

**Pergunta 4.2: Liste clientes em ordem alfabética de nome.**

SELECT idCliente, nome
FROM cliente
ORDER BY nome ASC;




**5. Agrupamentos com GROUP BY**

**Pergunta 5.1: Quantos produtos cada fornecedor possui cadastrado?**

SELECT f.idFornecedor,
       f.nome,
       COUNT(p.idProduto) AS total_produtos
FROM fornecedor f
LEFT JOIN produto p ON p.idFornecedor = f.idFornecedor
GROUP BY f.idFornecedor, f.nome;

**Pergunta 5.2: Total de itens vendidos por produto (em todos os pedidos).**

SELECT p.idProduto,
       p.nome,
       SUM(ip.quantidade) AS total_vendido
FROM produto p
JOIN itemPedido ip ON ip.idProduto = p.idProduto
GROUP BY p.idProduto, p.nome;




**6. Filtros em grupos com HAVING**

**Pergunta 6.1: Quais fornecedores têm mais de 50 produtos cadastrados?**

SELECT f.idFornecedor,
       f.nome,
       COUNT(p.idProduto) AS total_produtos
FROM fornecedor f
JOIN produto p ON p.idFornecedor = f.idFornecedor
GROUP BY f.idFornecedor, f.nome
HAVING COUNT(p.idProduto) > 50;

**Pergunta 6.2: Quais produtos já venderam ao menos 100 unidades?**

SELECT p.idProduto,
       p.nome,
       SUM(ip.quantidade) AS total_vendido
FROM produto p
JOIN itemPedido ip ON ip.idProduto = p.idProduto
GROUP BY p.idProduto, p.nome
HAVING SUM(ip.quantidade) >= 100;




**7. Junções (JOIN) entre tabelas**

**Pergunta 7.1: Para cada pedido, exibir nome do cliente, data do pedido e status.**

SELECT p.idPedido,
       c.nome      AS cliente,
       p.dataPed,
       p.statusPedido
FROM pedido p
JOIN cliente c ON c.idCliente = p.idCliente;

**Pergunta 7.2: Mostrar itens de um pedido (idPedido = 123), com nome do produto, quantidade e preço de venda.**

SELECT ip.idPedido,
       pr.nome         AS produto,
       ip.quantidade,
       ip.precoVenda
FROM itemPedido ip
JOIN produto pr ON pr.idProduto = ip.idProduto
WHERE ip.idPedido = 123;

**Pergunta 7.3: Listar pedidos com os dados de entrega (status e código de rastreio).**

SELECT p.idPedido,
       p.dataPed,
       e.statusEntrega,
       e.codigoRastreio
FROM pedido p
LEFT JOIN entrega e ON e.idPedido = p.idPedido;




**8. Subqueries**

**Pergunta 8.1: Quais clientes ainda não fizeram nenhum pedido?**

SELECT idCliente, nome
FROM cliente
WHERE idCliente NOT IN (
    SELECT DISTINCT idCliente
    FROM pedido
);

**Pergunta 8.2: Quais produtos têm estoque em lote vencido (dataValidade < hoje)?**

SELECT DISTINCT p.idProduto,
                p.nome
FROM produto p
JOIN estoque e ON e.idProduto = p.idProduto
WHERE e.dataValidade < CURDATE();




**9. Cenários de especialização disjunta (PF vs PJ)**

**Pergunta 9.1: Liste os clientes PF com CPF.**

SELECT c.idCliente, c.nome, pf.CPF
FROM cliente c
JOIN clientePF pf ON pf.idCliente = c.idCliente;

**Pergunta 9.2: Liste os clientes PJ com CNPJ e razão social.**

SELECT c.idCliente, c.nome, pj.CNPJ, pj.razaoSocial
FROM cliente c
JOIN clientePJ pj ON pj.idCliente = c.idCliente;

**Pergunta 9.3: Verificar se algum cliente está cadastrado tanto em PF quanto em PJ (não deveria ocorrer).**

SELECT c.idCliente, c.nome
FROM cliente c
JOIN clientePF pf ON pf.idCliente = c.idCliente
JOIN clientePJ pj ON pj.idCliente = c.idCliente;





 
