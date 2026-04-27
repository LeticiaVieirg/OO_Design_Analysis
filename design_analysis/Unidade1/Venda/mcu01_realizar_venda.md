## UC01 - Realizar Venda

### 1. Descrição

É um processo onde um atendente ou gerente realiza a venda de um produto do estoque para um cliente no estabelecimento da academia.

### 2. Importância

Baixa

### 3. Ator Primário / Ator Secundário

- Primário: Atendente

### 4. Pré-Condições

- Produto cadastrado no sistema;
- Produto disponível;
- Atendente precisa está autenticado


### 4. Pós-Condições

- Venda realizada ou cancelada;
- Nota fiscal emitida;
- Atualiza estoque;

### 5. Fluxo Principal

#### P1. Realizar Venda
##### P1.1 Atendente seleciona a opção "Realizar Venda";
##### P1.2 Sistema exibe a lista de produtos disponíveis;
##### P1.3 Atendente seleciona o produto; A1
##### P1.4 Atendente define a quantidade;
##### P1.5 Sistema solicita o CPF fo consumidor;
##### P1.6 Atendente informa o CPF;
##### P1.7 Atendente seleciona a opção "Finalizar";
##### P1.8 Sistema exibe as opções de pagamento;
##### P1.9 Atendente seleciona a opção de pagamento;
##### P1.10 Sistema solicita o pagamento;
##### P1.11 Sistema confirma pagamento;
##### P1.12 Caso de uso finalizado;


### 6. Fluxo Alternativo

### A1. Remover Produto;
#### A1.1 Atendente seleciona "Remover Produto"; P1.2

### A2. Cancelar Venda
#### A2.1 Atendente seleciona "Cancelar Venda"; P1.12

### 7. Fluxo de Exceção
### E1. Quantidade Errada
#### E1.1 Atendente seleciona a opção "Editar Venda"; P1.4

### E2. CPF inválido 
#### E2.1 Sistema exibe a mensagem "CPF inválido"

### 8. Regras de Negócio

- Atendente realiza venda de produtos do estoque;
- Vendas podem ser realizadas sem o CPF;

### Histórico

- Data: 19/03/2026 - Versão inicial - Responsável: Leticia;
- Data: 20/03/2026 - Atualização Regra de negócio - Responsável: Kayc
