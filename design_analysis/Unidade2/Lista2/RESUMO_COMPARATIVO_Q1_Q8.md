# Resumo Comparativo - Exercícios 1 a 8

## 📊 Visão Geral dos 8 Exercícios

```
Q1              Q2              Q3              Q4
SINTAXE        HERANÇA         INTERFACES      ASSOCIAÇÃO
├─ - privado   ├─ abstrata     ├─ Logavel      ├─ Professor 1
├─ # protegido ├─ concrete     ├─ impl         └─ Disciplina *
├─ + público   ├─ <|-- (1:N)   ├─ impl         └─ unidirecional
└─ getters     └─ polimorfismo └─ dependência

Q5              Q6              Q7              Q8
AGREGAÇÃO      STRATEGY        OBSERVER        FACTORY
├─ o-- (vazio) ├─ algoritmos   ├─ Subject      ├─ Creator
├─ independ.   ├─ context      ├─ Observer     ├─ Product
└─ Depto/Func  ├─ polimorfismo └─ 1:N notify   └─ ConcreteClasses
```

---

## 🎯 Questão 1: Sintaxe de Classe

### Conceitos
- **Visibilidade:** - (privado), # (protegido), + (público)
- **Atributos e Operações**
- **Getters/Setters**

### Diagrama
```
Produto
├─ - codigo: int
├─ - nome: String
├─ # preco: double = 0.0
├─ + emEstoque: boolean
├─ + getPreco(): double
├─ + setPreco(double): void
└─ - verificarDisponibilidade(): boolean
```

### Notação
```
- privado
# protegido
+ público
~ padrão
```

---

## 🏦 Questão 2: Classe Abstrata e Herança

### Conceitos
- **Herança:** <|--
- **Classe Abstrata:** {abstract}
- **Método Abstrato:** {abstract} + método()
- **Polimorfismo**

### Diagrama
```
         Conta (abstrata)
         ├─ # saldo
         ├─ {abstract} calcularTaxa()
         │
    ┌────┴────┐
    ▼         ▼
ContaCorrente ContaPoupanca
├─ limiteCheque ├─ taxaRendimento
└─ calcularTaxa() └─ calcularTaxa()
   (2% mês)        (0.5% mês)
```

### Notação
```
Conta <|-- ContaCorrente
      <|-- ContaPoupanca

{abstract} + método(): tipo
```

---

## 📝 Questão 3: Interfaces

### Conceitos
- **Interface:** <<interface>>
- **Realização:** <|..
- **Injeção de Dependência**
- **Acoplamento Fraco**

### Diagrama
```
        Logavel (interface)
        ├─ + log(String)
        │
    ┌───┴───┐
    ▼       ▼
LogArquivo LogBD
├─ arquivo ├─ conexão
└─ impl    └─ impl
    
    ServicoDeAplicacao
    ├─ - logger: Logavel
    └─ + ServicoDeAplicacao(Logavel)
```

### Notação
```
Logavel <|.. LogEmArquivo
            LogEmBancoDeDados
        
ServicoDeAplicacao --> Logavel
(dependência)
```

---

## 👨‍🏫 Questão 4: Associação e Navegabilidade

### Conceitos
- **Associação:** --
- **Multiplicidade:** 1, *, 0..*, 1..*
- **Navegabilidade Unidirecional:** <--
- **Não bidirecional**

### Diagrama
```
Professor          Disciplina
├─ matricula   ├─ codigo
├─ nome        ├─ nome
└─ lecionar()  ├─ professor
               └─ obterProfessor()

Professor 1 <-- 1..* Disciplina
(seta aponta para Professor)
```

### Notação
```
Professor "1" <-- "1..*" Disciplina

Apenas Disciplina → Professor
Professor ← Disciplina
```

---

## 🎁 Questão 5: Agregação vs. Composição

### Conceitos
- **Agregação:** o-- (losango vazio)
- **Composição:** *-- (losango cheio)
- **Ciclo de vida**
- **Exclusividade**

### Diagrama
```
AGREGAÇÃO:
Departamento 1 o-- 0..* Funcionario
├─ Func pode existir sem Depto
├─ Vida independente
└─ Pode compartilhar

COMPOSIÇÃO:
Pedido 1 *-- 1..* ItemDePedido
├─ Item não pode existir sem Pedido
├─ Vida dependente
└─ Pertence a apenas um Pedido
```

### Notação
```
Agregação:   o-- (vazio)
Composição:  *-- (cheio)
```

### Comparação
```
Agregação (fraca)     Composição (forte)
├─ tem um             ├─ é composto de
├─ independente       ├─ dependente
├─ pode compartilhar  └─ exclusivo
└─ Depto/Func         └─ Pedido/Item
```

---

## 🎯 Questão 6: Padrão Strategy

### Conceitos
- **Strategy Pattern**
- **Algoritmos intercambiáveis**
- **Context** usa interface
- **Concrete Strategies** implementam

### Diagrama
```
CompressorDeArquivos (Context)
├─ - algoritmo: AlgoritmoDeCompressao
├─ + setAlgoritmo(novo)
└─ + comprimirArquivo()
        │
        ├─ chamaAlgoritmo.comprimir()
        │
        └─ AlgoritmoDeCompressao (Interface)
           ├─ comprimir()
           ├─ descomprimir()
           │
    ┌──┬────────┬──┐
    ▼  ▼        ▼  ▼
   ZIP RAR    GZIP 7Z
```

### Notação
```
Context --> Strategy (interface)
        <|.. ConcreteStrategyA
        <|.. ConcreteStrategyB
```

### Vantagens
```
✓ Trocar algoritmos em runtime
✓ Fácil adicionar novos
✓ Evita if/else gigante
✓ Segue Open/Closed Principle
```

---

## 👀 Questão 7: Padrão Observer

### Conceitos
- **Observer Pattern**
- **Subject** notifica **Observers**
- **Dependência 1:N**
- **Loose coupling**

### Diagrama
```
ProdutoLeilao (Subject)
├─ - precoAtual
├─ - interessados: List<Interessado>
├─ + adicionarInteressado(i)
├─ + removerInteressado(i)
├─ + atualizarPreco(novo)
└─ - notificarInteressados()
        │
        └─ forEach(interessado)
           └─ interessado.atualizar(preco)
                │
    ┌───────────┼────────────┐
    ▼           ▼            ▼
Licitante Gerenciador Plataforma
  │         │          │
  └─────────┴──────────┘
    impl Interessado
```

### Notação
```
ProdutoLeilao "1" --> "0..*" Interessado
Interessado <|.. Licitante
           <|.. GerenciadorLeilao
           <|.. PlataformaMonitoramento
```

### Fluxo
```
1. atualizarPreco(2000.0)
2. notificarInteressados()
3. Para cada Interessado:
   └─ interessado.atualizar(2000.0)
4. Cada um reage de forma diferente
```

---

## 🏭 Questão 8: Padrão Factory Method

### Conceitos
- **Factory Method Pattern**
- **Creator** (abstrata) define factory
- **ConcreteCreators** implementam
- **Products** - objetos criados

### Diagrama
```
Documento (interface, Product)
├─ abrir()
├─ salvar()
├─ editar()
    │
    ├─ impl DocumentoDeTexto
    ├─ impl DocumentoDePlanilha
    └─ impl DocumentoDePresentacao

Editor (abstrata, Creator)
├─ {abstract} criarDocumento()
├─ novo()
├─ salvar()
    │
    ├─ EditorDeTexto → cria → DocumentoDeTexto
    ├─ EditorDePlanilha → cria → DocumentoDePlanilha
    └─ EditorDePresentacao → cria → DocumentoDePresentacao
```

### Notação
```
Documento <|.. DocumentoDeTexto
          <|.. DocumentoDePlanilha
          <|.. DocumentoDePresentacao

Editor <|-- EditorDeTexto
       <|-- EditorDePlanilha
       <|-- EditorDePresentacao

EditorDeTexto ..|> DocumentoDeTexto (cria)
```

### Fluxo
```
1. Editor editor = new EditorDeTexto()
2. editor.novo()
3. criarDocumento() é chamado
4. Retorna DocumentoDeTexto específico
5. Editor trabalha com Documento (interface)
```

---

## 🔀 Símbolos UML Utilizados

### Relacionamentos

```
Herança:           <|--
Realização:        <|..
Dependência:       -->
Associação:        --
Agregação:         o--
Composição:        *--
```

### Visibilidade

```
- privado (classe)
# protegido (classe + subclasses)
+ público (qualquer um)
~ padrão (mesmo pacote)
```

### Multiplicidade

```
1           exatamente um
*           zero ou mais
0..*        zero ou mais (explícito)
1..*        um ou mais
0..1        zero ou um
```

### Estereótipos

```
<<abstract>>      classe abstrata
<<interface>>     interface
<<create>>        criação
<<destroy>>       destruição
{abstract}        método abstrato
```

---

## 📈 Progressão de Complexidade

```
Q1: ⭐ Conceitos básicos
Q2: ⭐⭐ Herança e abstração
Q3: ⭐⭐ Interfaces e injeção
Q4: ⭐⭐ Navegabilidade
Q5: ⭐⭐⭐ Agregação/Composição
Q6: ⭐⭐⭐ Design Pattern (Strategy)
Q7: ⭐⭐⭐⭐ Design Pattern (Observer)
Q8: ⭐⭐⭐⭐ Design Pattern (Factory)
```

---

## 🎓 Mapa Mental dos Conceitos

```
MODELAGEM DE CLASSES
├─ FUNDAMENTAIS (Q1-Q4)
│  ├─ Sintaxe (Q1)
│  ├─ Herança (Q2)
│  ├─ Interfaces (Q3)
│  └─ Associações (Q4)
│
├─ RELACIONAMENTOS (Q5)
│  ├─ Agregação
│  └─ Composição
│
└─ PADRÕES (Q6-Q8)
   ├─ Strategy (Q6)
   ├─ Observer (Q7)
   └─ Factory (Q8)
```

---

## 🔑 Palavras-chave por Questão

| Q | Palavras-chave |
|---|---|
| 1 | Visibilidade, atributos, operações |
| 2 | Herança, abstração, polimorfismo |
| 3 | Interface, injeção, acoplamento fraco |
| 4 | Associação, navegabilidade, multiplicidade |
| 5 | Agregação, composição, ciclo de vida |
| 6 | Strategy, algoritmos, intercambiável |
| 7 | Observer, notificação, 1:N |
| 8 | Factory, criação, abstração |

---

## 💡 Quando Usar Cada Padrão

### Strategy (Q6)
**Quando:** Múltiplos algoritmos para mesma tarefa
**Exemplo:** Compressão, Ordenação, Pagamento
**Benefício:** Trocar algoritmo em runtime

### Observer (Q7)
**Quando:** 1 objeto notifica muitos
**Exemplo:** Leilão, Events, MVC
**Benefício:** Acoplamento fraco

### Factory (Q8)
**Quando:** Criar diferentes tipos
**Exemplo:** Documentos, Conexões, Drivers
**Benefício:** Centraliza criação

---

## 📝 Exercício Prático Integrado

Combinar os conceitos:

```
1. Criar classe Comprador (Q1)
2. Herdar de Pessoa (Q2)
3. Usar interface Notificavel (Q3)
4. Associar com Pedido (Q4)
5. Agregar Endereço (Q5)
6. Factory para criar Pedido (Q8)
7. Observer para mudanças de status (Q7)
```

Resultado: Sistema de e-commerce robusto!

