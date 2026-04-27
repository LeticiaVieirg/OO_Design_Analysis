
## UC02 - Cadastrar Treino

### 1. Descrição

É um processo onde um Instrutor ou gerente cadastra o treino no estabelecimento da academia.

### 2. Importância

Alta

### 3. Ator Primário / Ator Secundário

- Primário: Instrutor

### 4. Pré-Condições

- Treino cadastrado com sucesso;
- Treino disponível;
- Instrutor precisa está autenticado;

### 4. Pós-Condições

- Treino concluido ou cancelado;
- Atualiza treino;

### 5. Fluxo Principal

#### P1. Cadastrar Treino

##### P1.1 Instrutor seleciona a opção "Cadastrar Treino"

##### P1.3 Sistema solicita os dados do cliente

##### P1.4 Sistema exibe a lista de exercícios disponíveis

##### P1.5 Instrutor seleciona os exercícios

##### P1.6 Instrutor define a carga

##### P1.7 Instrutor seleciona a opção "Finalizar"

##### P1.8 Sistema exibe o treino relacionado ao cliente

##### P1.9 Sistema confirma dados

##### P1.10 Caso de uso finalizado

### 6. Fluxo Alternativo

### A1. Treino Pré-Cadastrado

#### A1.1 Instrutor seleciona "Tipo de Treino"; P1.5

### A2. Cancelar treino

#### A2.1 Instrutor seleciona "Cancelar Treino"; P1.12

### 7. Fluxo de Exceção

### E1. Exercício com carga Errada

#### E1.1 Instrutor seleciona a opção "Editar Treino"; P1.6

### 8. Regras de Negócio

- Instrutor realiza treino de clientes do estabelecimento;
- 

### Histórico

- Data: 19/03/2026 - Versão inicial - Responsável: Leticia;
- Data: 20/03/2026 - Atualização Regra de negócio - Responsável: Kayc;