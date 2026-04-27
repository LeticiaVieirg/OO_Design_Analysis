## UC02 - Cadastrar Treino

### 1. Descrição

Este caso de uso ocorre quando um Instrutor tenta cadastrar um treino no sistema para um Cliente específico ou para treinos padronizados.

### 2. Importância

Alta.

### 3. Ator Primário / Ator Secundário

- Primário: Instrutor

### 4. Pré-condições

- Instrutor autenticado no sistema;
- Exercícios e máqunias pré-cadastrados no sistema;

### 5. Pós-condições

- Treino cadastrado no sistema.

### 6. Fluxo Principal

#### P1 Cadastrar Treino
##### P1.1 Instrutor seleciona a opção "Cadastrar Treino";
##### P1.2 Sistema exibe a lista de exercícios cadastrados;
##### P1.3 Instrutor adiciona exercício ao treino; A1
##### P1.4 Sistema solicita a carga; A1 A2
##### P1.5 Instrutor seleciona a opção "Selecionar Cliente";
##### P1.6 Sistema exibe campo de busca para Cliente;
##### P1.7 Instrutor informa nome do Cliente; E1
##### P1.8 Sistema exibe a lista de Clientes encontrados;
##### P1.9 Instrutor seleciona Cliente;
##### P1.10 Instrutor seleciona a opção "Finalizar Cadastro"; 
##### P1.11 Sistema exibe a mensagem "Novo treino cadastrado!";
##### P1.12 Sistema retorna para tela inicial;
##### P1.13 Caso de uso finalizado.


### 7. Fluxo Alternativo

#### A1. Adicionar mais exercícios
##### A1.1 Instrutor seleciona a opção "Adicionar Exercício"; P1.3

#### A2. Adicionar Modelo de Treino
##### A2.1 Instrutor seleciona a opção "Modelo Treino";
##### A2.2 Sistema solicita um nome para o modelo; P1.10

### 8. Fluxo de Exceção

#### E1. Cliente não encontrado

##### E1.1 Sistema exibe a mensagem "Cliente não encontrado"; P1.6

### 9. Regras de Negócio

- O treino será cadastrado para um Cliente após a avaliação física;
- Haverá um Instrutor responsável pelo treino de cada Cliente;

### Histórico

- Data: 19/03/2026 - Versão Inicial - Responsável: Ferdinandy Chagas