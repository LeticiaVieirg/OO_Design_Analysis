# Consulta de Saldo com Autenticação Opcional

## 📋 Visão Geral

Este documento descreve o fluxo de uma aplicação bancária onde um usuário consulta seu saldo. O sistema implementa dois caminhos:

1. **Caminho Rápido**: Se o usuário já está autenticado, o saldo é retornado diretamente
2. **Caminho Completo**: Se o usuário não está autenticado, primeiro realiza login, depois consulta o saldo

---

## 🔄 Fluxo Simplificado (Diagrama 1)

### Caso 1: Usuário Já Autenticado ✓

```
Usuario → App: consultarSaldo()
                ↓
          [Verifica sessão ativa?]
                ↓ SIM
          Recupera token da sessão
                ↓
          Servidor: buscarSaldo(token)
                ↓
          Valida token no servidor
                ↓
          Consulta BD
                ↓
          Retorna saldo ao usuário
```

**Responsáveis:**
- **AppBancario**: Gerencia a interface e coordena as requisições
- **ServidorSaldo**: Valida o token e busca o saldo
- **SessaoUsuario**: Armazena o token de autenticação

**Tempo de resposta:** ~200-500ms (sem autenticação)

---

### Caso 2: Usuário Não Autenticado ✗

```
Usuario → App: consultarSaldo()
                ↓
          [Verifica sessão ativa?]
                ↓ NÃO
          Exibe tela de login
                ↓
          Usuario: insere CPF e senha
                ↓
          ServidorAutenticacao: valida credenciais
                ↓
          Cria token de sessão
                ↓
          Armazena token localmente
                ↓
          Servidor: buscarSaldo(token)
                ↓
          Retorna saldo ao usuário
```

**Responsáveis:**
- **AppBancario**: Interface e coordenação
- **ServidorAutenticacao**: Valida credenciais e cria token
- **ServidorSaldo**: Busca saldo com token válido
- **SessaoUsuario**: Armazena sessão para próximas requisições

**Tempo de resposta:** ~1-2s (com autenticação)

---

## 🔐 Autenticação Detalhada (Diagrama 2)

### Estrutura de Tokens

```
Token JWT:
┌─────────────┬─────────────┬─────────────┐
│   Header    │   Payload   │  Signature  │
├─────────────┼─────────────┼─────────────┤
│ Algorithm   │ User Claims │ Secret Key  │
│ Type        │ Expiration  │             │
│             │ Issue Date  │             │
└─────────────┴─────────────┴─────────────┘

Exemplo:
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### Fluxo de Verificação de Sessão

```
┌─────────────────────────────────┐
│ Usuario clica "Consultar Saldo" │
└──────────────┬──────────────────┘
               ↓
    ┌──────────────────────────┐
    │ App verifica sessão ativa│
    └────────────┬─────────────┘
                 ↓
         ┌───────────────┐
         │ Sessão existe?│
         └───────┬───────┘
                 │
         ┌───────┴────────┐
         ↓                ↓
      SIM              NÃO
      │                │
      ↓                ↓
   Token            Login
   válido?          requerido
      │
      ├─ SIM: Busca saldo direto
      └─ NÃO: Solicita reautenticação
```

### Validação de Credenciais

```
CPF e Senha inseridos
        ↓
Sanitização de entrada
└─ Remove espaços
└─ Valida formato CPF
└─ Verifica tamanho da senha
        ↓
Busca usuário no BD
        ↓
┌─── Usuário existe? ───┐
│                       │
NÃO                   SIM
│                       │
└─ Erro              Comparar senha
   enviado              │
                        ├─ bcrypt.compare()
                        ├─ Argon2.verify()
                        ↓
                    ┌─Senha OK─┐
                    │          │
                   SIM        NÃO
                    │          │
                    ↓          ↓
                 Gerar      Registrar
                 Token      tentativa
                    │       falha
                    └─ Bloqueia
                       após 3
                       tentativas
```

---

## 🛡️ Tratamento de Exceções (Diagrama 3)

### Exceções Possíveis

```
┌─────────────────────────────────┐
│  EXCEÇÕES DURANTE AUTENTICAÇÃO  │
└─────────────────────────────────┘

1. TokenExpiradoException
   └─ Token venceu
   └─ Solução: Solicitar novo login

2. TokenInvalidoException
   └─ Token corrompido/adulterado
   └─ Solução: Sessão comprometida

3. CredenciaisInvalidasException
   └─ CPF ou senha incorretos
   └─ Solução: Exibir mensagem de erro

4. UsuarioNaoEncontradoException
   └─ CPF não existe na base
   └─ Solução: Registrar tentativa, bloquear após N tentativas

5. ContaBloqueadaException
   └─ Usuário fez muitas tentativas falhas
   └─ Solução: Desbloquear após X minutos

6. DatabaseException
   └─ Erro ao conectar ao BD
   └─ Solução: Exibir mensagem genérica (nunca revelar detalhes)

7. TimeoutException
   └─ Servidor levou muito tempo
   └─ Solução: Retry automático com backoff exponencial
```

### Fluxo com Tratamento de Erros

```
Try {
    Validar credenciais
    Gerar token
    Buscar saldo
} Catch (CredenciaisInvalidasException) {
    Log: Tentativa falha
    Response: "Credenciais inválidas"
    Bloquear após 3 tentativas
}
Catch (DatabaseException) {
    Log: Erro crítico
    Response: "Erro de conexão. Tente novamente."
}
Catch (Exception) {
    Log: Erro desconhecido
    Response: "Erro inesperado. Contate o suporte."
}
```

---

## 🔒 Medidas de Segurança

### 1. Autenticação
- ✓ Hash de senha com bcrypt ou Argon2
- ✓ Validação de credenciais no servidor
- ✓ Sem armazenamento de senhas em texto plano

### 2. Autorização
- ✓ Token com expiração (30 minutos)
- ✓ Validação de token em cada requisição
- ✓ Escopo de acesso limitado (apenas saldo do próprio usuário)

### 3. Prevenção de Ataques
- ✓ Sanitização de entrada (SQL injection)
- ✓ Rate limiting (máx 3 tentativas por IP)
- ✓ Bloqueio de conta após falhas
- ✓ Logging e auditoria de acessos

### 4. Transporte
- ✓ HTTPS em todas as comunicações
- ✓ Token não trafega em URL (apenas headers)
- ✓ CORS restrictivo

---

## 📊 Comparação de Caminhos

### Caminho 1: Usuário Autenticado

| Aspecto | Detalhes |
|---------|----------|
| **Sequência de passos** | 4-5 passos |
| **Tempo aproximado** | 200-500ms |
| **Verificações** | Token válido, não expirado |
| **Chamadas ao BD** | 1 (apenas buscar saldo) |
| **Camadas envolvidas** | App → Sessão → Servidor → BD |
| **Experiência do usuário** | Rápida e transparente |

### Caminho 2: Usuário Não Autenticado

| Aspecto | Detalhes |
|---------|----------|
| **Sequência de passos** | 10-12 passos |
| **Tempo aproximado** | 1-2 segundos |
| **Verificações** | Credenciais, geração token |
| **Chamadas ao BD** | 2 (buscar usuário, buscar saldo) |
| **Camadas envolvidas** | App → Auth → BD → Sessão → Servidor → BD |
| **Experiência do usuário** | Mais lenta (login necessário) |

---

## 📈 Sequência Numérica (Autonumeração)

### Exemplo: Usuário Autenticado

```
1: consultarSaldo()
   1.1: verificarAutenticacao()
   1.2: obterTokenSessao()
   1.3: buscarSaldo(token)
      1.3.1: validarToken()
      1.3.2: consultarBancoDados()
2: exibirSaldo()
```

### Exemplo: Usuário Não Autenticado

```
1: consultarSaldo()
   1.1: verificarAutenticacao() → false
2: solicitarAutenticacao()
3: inserirCredenciais()
   3.1: autenticar()
      3.1.1: validarCredenciais()
      3.1.2: criarSessao()
   3.2: buscarSaldo()
      3.2.1: validarToken()
      3.2.2: consultarBancoDados()
4: exibirSaldo()
```

---

## 🎯 Casos de Uso

### 1. Primeiro Acesso do Dia

```
Usuário abre app → Sessão expirada → Login → Consulta saldo
```

### 2. Consulta Rápida (Mesmo Dia)

```
Usuário abre app → Sessão válida → Consulta saldo (sem login)
```

### 3. Senha Esquecida

```
Usuário → Credenciais inválidas → Erro → Link "Recuperar senha"
```

### 4. Conta Bloqueada

```
Usuário → 3 tentativas falhas → Conta bloqueada → Aguardar desbloqueio
```

---

## 🔗 Componentes e Responsabilidades

### AppBancario (Frontend)
- ✓ Interface com usuário
- ✓ Gerencia formulários de login
- ✓ Armazena sessão localmente
- ✓ Valida entrada do usuário
- ✓ Trata exceções e exibe mensagens

### GerenciadorSessao
- ✓ Verifica se sessão está ativa
- ✓ Recupera/armazena tokens
- ✓ Verifica expiração
- ✓ Limpa sessão ao logout

### ServidorAutenticacao
- ✓ Valida credenciais
- ✓ Gera tokens JWT
- ✓ Registra tentativas falhas
- ✓ Bloqueia contas suspeitas

### ServidorSaldo
- ✓ Valida token
- ✓ Consulta saldo do usuário
- ✓ Formata resposta
- ✓ Registra acessos em log

### BancoDados
- ✓ Armazena usuários
- ✓ Armazena contas/saldos
- ✓ Registra tentativas falhas
- ✓ Mantém log de auditoria

---

## 💾 Dados Armazenados Localmente (Cache Token)

```
┌──────────────────────────────┐
│     Cache Token Local        │
├──────────────────────────────┤
│ idUsuario: "12345"           │
│ token: "eyJhbGci..."         │
│ expiracaoEm: 1234567890      │
│ ipOrigem: "192.168.1.100"    │
│ browserInfo: "Mozilla..."    │
└──────────────────────────────┘
```

---

## ⏱️ Timing e Performance

### Operação Bem-sucedida (Autenticado)

```
T0:     Usuario clica "Consultar Saldo"
T+100ms: App verifica sessão
T+150ms: Recupera token do cache
T+200ms: Envia requisição ao Servidor
T+350ms: Servidor valida token
T+400ms: Servidor consulta BD
T+450ms: Resposta retorna
T+500ms: Saldo exibido ao usuário
```

### Operação com Login

```
T0:     Usuario clica "Consultar Saldo"
T+100ms: App verifica sessão → ausente
T+150ms: Exibe tela de login
T+500ms: Usuario preenche formulário
T+700ms: Envia credenciais
T+800ms: Servidor busca usuário
T+900ms: Servidor compara senha
T+950ms: Servidor gera token
T+1000ms: Token armazenado localmente
T+1100ms: Requisição de saldo enviada
T+1300ms: Saldo consultado
T+1400ms: Resposta retorna
T+1500ms: Saldo exibido
```

---

## 🚀 Otimizações Possíveis

1. **Cache do Saldo Local**
   - Armazenar saldo último consultado
   - Refresh automático a cada 5 minutos
   - Símbolo de "dados antigos" se > 10 min

2. **Refresh Token Automático**
   - Renovar token 5 minutos antes de expirar
   - Operação invisível ao usuário
   - Sem necessidade de novo login

3. **Biometria**
   - Face ID / Fingerprint em vez de senha
   - Apenas para unlock local (token seguro no servidor)

4. **Progressive Web App (PWA)**
   - Funcionar offline com dados em cache
   - Sincronizar quando houver conexão

5. **API de Alta Performance**
   - GraphQL em vez de REST (consultar apenas saldo)
   - Compressão GZIP
   - CDN para reduzir latência

