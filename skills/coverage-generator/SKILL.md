---
name: coverage-generator
description: Gera código TLPP de cobertura automática para fontes AdvPL/TLPP, criando arquivo `coverage/coverage.<nome>.tlpp` com classes de teste numeradas e `coverage/coverage.<nome>.md` com pré-requisitos. Cada método numerado (m001, m002...) cobre uma linha ou branch específica do arquivo original. Use quando precisar "gerar cobertura para src/class1.tlpp", "create test coverage for TLPP", "generate coverage code", "cobertura automática".
---

# Coverage Generator Skill

Gera automaticamente código de cobertura de teste para arquivos TLPP, facilitando validação de cada linha e branch do código-fonte através de métodos públicos numerados e rastreáveis.

## Propósito

Automatizar a criação de arquivos de cobertura que:
- Executam **cada linha e branch** do código original pelo menos uma vez
- Organizam métodos **numerados sequencialmente** (m001, m002, m003...) para fácil rastreamento
- Documentam **pré-requisitos de dados** e **limitações de cobertura** em arquivo `.md`
- Usam **TLPP puro** (sem dependências de framework)
- Facilitam **validação manual** em ambiente controlado de teste

## Quando usar

- "Gere cobertura para `src/contracts/adjustment/validator.tlpp`"
- "Crie arquivo de teste para cobrir `src/custom.class.tlpp`"
- "Quero um arquivo que cobre cada linha de `src/Billing.tlpp`"
- "Gere métodos numerados para testar todas as branches"

## Como funciona

1. **Entrada**: Caminho de arquivo TLPP (ex: `src/MyClass.tlpp`)
2. **Análise**: Parse TLPP nativo extrai classes, métodos, blocos, condicionais
3. **Geração**: Cria método numerado para cada linha/branch a cobrir
4. **Documentação**: Gera arquivo `.md` com pré-requisitos e limitações
5. **Saída**: Dois arquivos criados:
   - `coverage/coverage.MyClass.tlpp` — código TLPP puro
   - `coverage/coverage.MyClass.md` — documentação executável

## Exemplo de uso

**Entrada:**
```
Gere cobertura para src/BillingValidator.tlpp
```

**Saída esperada:**

### Arquivo: `coverage/coverage.BillingValidator.tlpp`

```tlpp
using namespace coverage

class CoverageBillingValidator
    public method m001() as logical
    public method m002() as logical
    ...
    public static method runAllTests() as logical
endclass

method m001() as logical class CoverageBillingValidator
    // Cobre linhas 15-18: validação de cliente existente
    local oValidator := BillingValidator():new()
    local lResult := oValidator:validateClient("CLI001")
    return iif(lResult, .T., .F.)
endmethod

method m002() as logical class CoverageBillingValidator
    // Cobre linha 20: branch if cliente inativo (False)
    local oValidator := BillingValidator():new()
    local lResult := oValidator:validateClient("CLI_INATIVO")
    return iif(lResult, .T., .F.)
endmethod

// ... mais métodos

method runAllTests() as logical class CoverageBillingValidator
    local lResult := .T.
    local nCount := 0
    
    if !::m001()
        conout("Coverage - m001() FALHOU")
        lResult := .F.
    else
        conout("Coverage - m001() executado com sucesso")
        nCount++
    endif
    
    if !::m002()
        conout("Coverage - m002() FALHOU")
        lResult := .F.
    else
        conout("Coverage - m002() executado com sucesso")
        nCount++
    endif
    
    conout("Coverage - Total: " + cvaltochar(nCount) + " métodos executados")
    return lResult
endmethod
```

### Arquivo: `coverage/coverage.BillingValidator.md`

```markdown
# Cobertura — BillingValidator

## Resumo

Gerado automaticamente. Arquivo original: `src/BillingValidator.tlpp`

## Métodos públicos de cobertura

- `m001()` — Cobre linhas 15-18 (validação cliente válido)
- `m002()` — Cobre linhas 20 (validação cliente inativo)
- ...

## Pré-requisitos por método

### m001()
- Adicione cliente TEST001 em tabela SA1 com status ATIVO

### m002()
- Adicione cliente TEST_INATIVO em tabela SA1 com status INATIVO

...

## Limitações de cobertura

- Linha 45: `Runtime Error "Arquivo não encontrado"` — não testável
```

## Recursos principais

- ✅ **Cobertura de linha** — cada linha executada ≥1 vez
- ✅ **Cobertura de decisão** — branches True/False em if/else
- ✅ **Métodos numerados** — m001(), m002()... em sequência
- ✅ **Rastreabilidade** — cada método indica exatamente qual trecho cobre
- ✅ **Classes derivadas** — acesso a métodos privados/protegidos via herança
- ✅ **Mocks simples** — simulação básica de APIs externas
- ✅ **Documentação executável** — pré-requisitos claros no `.md`
- ✅ **runAllTests()** — executar todos os testes em sequência
- ✅ **Log mínimo** — conout() para rastreamento básico

## Limitações e escopo

**Não cobre:**
- Cobertura de todos os caminhos (path coverage) — apenas linha + branch
- Integração com AdvPR/FWTestHelper — TLPP puro standalone
- Integração com TIR/Python — sem testes de interface
- Compilação/execução automática — apenas geração de código
- Análise de dependências externas — apenas arquivo fornecido

**Assume:**
- Ambiente de teste controlado (não produção)
- Arquivo TLPP válido (Protheus 12.1.2510+)
- Namespace `coverage` disponível
- Possibilidade de alterar parâmetros/dados de teste

## Fluxo de uso típico

1. Indique arquivo: "Gere cobertura para `src/MyClass.tlpp`"
2. Skill valida arquivo e analisa estrutura
3. Skill gera método para cada linha/branch
4. Skill cria arquivo `.tlpp` em `coverage/` com classes + runAllTests()
5. Skill cria arquivo `.md` com instruções de pré-requisitos
6. Você executa `runAllTests()` em ambiente Protheus
7. Você verifica logs (conout) e cumpre pré-requisitos conforme documentado

## Integração com projeto

- **Namespace obrigatório**: `coverage` — nunca altera namespace do original
- **Convenções**: Segue [development.instructions.md](../../.github/instructions/development.instructions.md)
- **Linguagem gerada**: TLPP puro (Protheus 12.1.2510+)
- **Stack**: AdvPL/TLPP apenas
- **Português**: Comentários e Protheus.doc em português; Markdown documentação em português

## Referência técnica

Veja `specs/coverage/coverage.spec.md` para especificação completa, requisitos funcionais e critérios de aceitação.

