# Especificação - Caso de Uso - Realizar Inscrição

## 1. Descrição
Trata-se do processo em que um usuario deseja realizar sua inscrição em um evento
Para poder garantir sua participação em atividades e oficinas envolvidas

## 2. Importancia
Alta

## 3. Atores
Primário: Participante

## 4. Tipo de Caso de Uso 
Concreto

## 5. Pré-Condições
O participante deve esta cadastrado no sistema

## 6. Pós-Condições
Participante inscrito no evento com comprovante de inscrição enviado por email;

## 7. Fluxo Principal
### P01.Realizar Inscrição
P1.1 O participante seleciona o evento;
P1.2 O participante seleciona a opção "Realizar inscrição"; A1.1
P1.3 O sistema exibe formulário para preencher os dados da inscrição;
P1.4 Participante preenche dados do formulario; A1.1
P1.5 Participante seleciona a opção "Cadastrar";
P1.6 Sistema confirma a incrição com a mensagem "Inscrição Realizada"
P1.7 Sistema gera notificação por email com os dados da inscrição;
P1.8 Caso de uso encerrado.

## 8. Fluxo Alternativo
### A01. Cancelar Inscrição
A1.1 O participante seleciona a opção "Cancelar" ; P1.8

## 9. Fluxo Exceção (Quando algo da errado)
### E01. Vagas Preenchidas
E1.1 Sistema exibe mensagem "Vagas esgotadas!";
E1.2 Sistema exibe mensagem "Entrar em lista de espera?"
E1.3 Participante seleciona a opção "Sim";
E1.4 Sistema exibe a mensagem "Adicionado a lista de espera"; P1.8

## 10. Regra de Negócio 