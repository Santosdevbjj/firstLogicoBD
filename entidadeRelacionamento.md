## Modelo de Entidade de Relacionamento de Ecommerce.


**Abaixo o modelo de Entidade de Relacionamento:**

![ecommerce016](https://github.com/user-attachments/assets/7b33380a-8839-4331-9a0d-793af4e89a0b)



## Tabelas: 

  **fornecedor:**
    idFornecedor (INT, PK): Identificador único do fornecedor.
   nome (STRING): Nome do fornecedor.
   endereco (STRING): Endereço do fornecedor.
   contato (STRING): Nome da pessoa de contato no fornecedor.
   email (STRING): Email do fornecedor.
   telefone (STRING): Telefone do fornecedor.
   dataFor (DATE): Data de cadastro do fornecedor.
   **Relacionamento: 1:n com produto e estoque.**



 **produto:**
   idProduto (INT, PK): Identificador único do produto.
   nome (STRING): Nome do produto.
   descricao (STRING): Descrição do produto.
   preco (FLOAT): Preço do produto.
    idFornecedor (INT, FK): Identificador do fornecedor do produto.
    dataProd (DATE): Data de cadastro do produto.
    statusProduto (ENUM): Status do produto (ativo, inativo, descontinuado).
   imagemProduto (STRING): Url da imagem do produto.
    pesoProduto (FLOAT): Peso do produto.
   **Relacionamento: 1:n com estoque, produtoCategoria e produtoPromocao.**



 **estoque:**
    idEstoque (INT, PK): Identificador único do estoque.
    quantidade (INT): Quantidade em estoque.
    localizacao (STRING): Localização do produto no estoque.
    idProduto (INT, FK): Identificador do produto em estoque.
    dataEsto (DATE): Data de cadastro do estoque.
    dataValidade (DATE): Data de validade do produto (se aplicável).
    numeroLote (STRING): Número do lote do produto.
    **Restrição: Combinação única de idProduto e numeroLote.**
    **Relacionamento: n:1 com produto.**



  **cliente:**
    idCliente (INT, PK): Identificador único do cliente.
   nome (STRING): Nome do cliente.
   email (STRING): Email do cliente.
    telefone (STRING): Telefone do cliente.
   dataCli (DATE): Data de cadastro do cliente.
    **Relacionamento: 1:n com pedido, clientePF, clientePJ e clienteEndereco.**


  **endereco:**
    idEndereco (INT, PK): Identificador único do endereço.
   logradouro (STRING): Logradouro do endereço.
   numero (STRING): Número do endereço.
   complemento (STRING): Complemento do endereço.
    bairro (STRING): Bairro do endereço.
    cidade (STRING): Cidade do endereço.
   estado (STRING): Estado do endereço.
   CEP (STRING): CEP do endereço.
    **Relacionamento: 1:n com clienteEndereco.**



 **clienteEndereco:**
    idClienteEndereco (INT, PK): Identificador único do relacionamento cliente-endereço.
    idCliente (INT, FK): Identificador do cliente.
   idEndereco (INT, FK): Identificador do endereço.
   tipoEndereco (ENUM): Tipo de endereço (cliente, entrega, cobrança).
    **Relacionamento: n:1 com cliente e endereco.**



  **categoria:**
    idCategoria (INT, PK): Identificador único da categoria.
    nomeCategoria (STRING): Nome da categoria.
    descricaoCategoria (STRING): Descrição da categoria.
    **Relacionamento: 1:n com produtoCategoria.**



 **produtoCategoria:**
    idProduto (INT, FK): Identificador do produto.
    idCategoria (INT, FK): Identificador da categoria.
    Chave Primária Composta: idProduto e idCategoria.
   **Relacionamento: n:1 com produto e categoria.**



 **promocao:**
   idPromocao (INT, PK): Identificador único da promoção.
   nomePromocao (STRING): Nome da promoção.
   descricaoPromocao (STRING): Descrição da promoção.
   dataInicio (DATE): Data de início da promoção.
   dataFim (DATE): Data de fim da promoção.
    tipoDesconto (STRING): Tipo de desconto (percentual, valor fixo).
    valorDesconto (FLOAT): Valor do desconto.
   **Relacionamento: 1:n com produtoPromocao.**



 **produtoPromocao:**
   idProduto (INT, FK): Identificador do produto.
    idPromocao (INT, FK): Identificador da promoção.
    Chave Primária Composta: idProduto e idPromocao.
   **Relacionamento: n:1 com produto e promocao.**


  **clientePF:**
    idCliente (INT, FK, PK): Identificador do cliente (pessoa física).
    CPF (STRING): CPF do cliente.
    **Relacionamento: 1:1 com cliente.**



  **clientePJ:**
    idCliente (INT, FK, PK): Identificador do cliente (pessoa jurídica).
    CNPJ (STRING): CNPJ do cliente.
    razaoSocial (STRING): Razão social do cliente.
    **Relacionamento: 1:1 com cliente.**



 **pedido:**
    idPedido (INT, PK): Identificador único do pedido.
   dataPed (DATE): Data do pedido.
   statusPedido (ENUM): Status do pedido (pendente, enviado, cancelado, entregue).
   valorFrete (FLOAT): Valor do frete do pedido.
   idCliente (INT, FK): Identificador do cliente que fez o pedido.
   **Relacionamento: 1:n com itemPedido, 1:1 com entrega e pagamento, n:1 com cliente.**



  **itemPedido:**
   idPedido (INT, FK): Identificador do pedido.
    idProduto (INT, FK): Identificador do produto no pedido.
    quantidade (INT): Quantidade do produto no pedido.
   precoVenda (FLOAT): Preço de venda do produto no pedido.
    idEstoque (INT, FK): Identificador do estoque do produto no pedido.
   Chave Primária Composta: idPedido e idProduto.
    **Relacionamento: n:1 com pedido, produto e estoque.**



  **entrega:**
    idEntrega (INT, PK): Identificador único da entrega.
    statusEntrega (ENUM): Status da entrega (em trânsito, entregue).
   codigoRastreio (STRING, UNIQUE): Código de rastreio da entrega.
   idPedido (INT, FK): Identificador do pedido da entrega.
   dataEntr (DATE): Data da entrega.
    **Relacionamento: 1:1 com pedido.**



 **pagamento:**
    idPagamento (INT, PK): Identificador único do pagamento.
   tipoPagamento (STRING): Tipo de pagamento (cartão de crédito, carta de débito, boleto, dinheiro).
    detalhes (STRING): Detalhes do pagamento.
    idPedido (INT, FK): Identificador do pedido do pagamento.
   dataPag (DATE): Data do pagamento.
    **Relacionamento: 1:1 com pedido.**


**Especialização Disjunta de Cliente:**
 A tabela cliente é a tabela pai para as especializações clientePF e clientePJ.

 A chave primária idCliente da tabela cliente é usada como chave estrangeira nas tabelas clientePF e clientePJ.

 **Um cliente só pode ser do tipo clientePF ou clientePJ, mas não ambos ao mesmo tempo.** Isso é garantido pela restrição de chave estrangeira e pela lógica da aplicação.







 
