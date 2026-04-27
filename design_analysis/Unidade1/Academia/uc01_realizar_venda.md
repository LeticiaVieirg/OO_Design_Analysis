## UC01 - Realizar Venda

### 1. Descrição

É o processo onde um Atendente ou Gerente realiza a venda de um Produto do estoque para um Cliente no estabelecimento da academia.

### 2. Importância

Baixa

### 3. Ator Primário / Ator Secundário

- Primário: Atendente

### 4. Pré-condições

- Produto cadastrado no sistema;
- Produto disponível;
- Atendente precisa estar autenticado no sistema;

### 4. Pós-condições

- Venda realizada ou cancelada;
- Nota fiscal emitida;
- Estoque atualizado.

### 5. Fluxo Principal

#### P1. Realizar Venda
##### P1.1 Atendente seleciona a opção "Realizar Venda";
##### P1.2 Sistema exibe a lista de produtos disponíveis;
##### P1.3 Atendente seleciona o produto; A1
##### P1.4 Atendente define a quantidade;
##### P1.5 Sistema solicita CPF do consumidor; A3
##### P1.6 Atendente informa CPF; E2
##### P1.7 Atendente seleciona a opção "Finalizar"; E1
##### P1.8 Sistema exibe as opções de formas de pagamento; A2
##### P1.9 Atendente seleciona a forma de pagamento;
##### P1.10 Sistema solicita pagamento;
##### P1.11 Sistema confirma pagamento;
##### P1.12 Caso de uso finalizado.

### 6. Fluxo Alternativo

#### A1. Remover Produto
##### A1.1 Atendente seleciona "Remover Produto"; P1.2

#### A2. Cancelar Venda
##### A2.1 Atendente seleciona "Cancelar Venda"; P1.12

#### A3. Sem CPF
##### A3.1 Atendente seleciona "Sem CPF"; P1.7

### 7. Fluxo de Exceção

#### E1. Quantidade Errada
##### E1.1 Atendente seleciona a opção "Editar Venda"; P1.4

#### E2. CPF inválido
##### E2.1 Sistema exibe a mensagem "CPF Inválido!"; P1.5

#### E3. Falha no Pagamento
##### E3.1 Sistema exibe a mensagem "Falha ao tentar realizar pagamento!"; P1.8

### 8. Regras de Negócio

#### RN01 - Atendente realiza venda de produtos do estoque;
#### RN02- São permitidos pagamentos em dinheiro, pix e cartão;
#### RN03- As vendas devem ser registradas;
#### RN04- As vendsa devem emitir nota fiscal;
#### RN05- Vendas podem ser realizadas sem CPF;
#### RN06- Quando uma venda for realizada o estoque deve ser atualizado;
#### RN07- Vendas somente podem ser realizadas por Atendentes.

-------------------------------------
### Histórico

- Data: 19/03/2026 - Versão Inicial - Responsável: Ferdinandy Chagas
- Data: 20/03/2026 - Atualização das Regras de Negócio - Responsável: Daniel Neres
