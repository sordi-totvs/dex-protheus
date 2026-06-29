# Cobertura — {{FILENAME}}

## Resumo

Gerado automaticamente. Arquivo original: `{{ORIGINAL_PATH}}`

| Propriedade | Valor |
|---|---|
| Data de geração | {{DATE}} |
| Classe principal | `Coverage{{CLASS_NAME}}` |
| Total de métodos | {{TOTAL_METHODS}} |
| Namespace | `coverage` |

## Métodos públicos de cobertura

{{METHODS_LIST}}

## Executar a cobertura

### Opção 1: Todos os testes em sequência

```tlpp
Coverage{{CLASS_NAME}}:runAllTests()
```

Este método executa todos os m001, m002... em sequência e retorna `.T.` se todos passarem, `.F.` se algum falhar.

### Opção 2: Teste individual

```tlpp
local oCoverage := Coverage{{CLASS_NAME}}():new()
local lResult := oCoverage:m001()
conout("Resultado: " + iif(lResult, "SUCESSO", "FALHA"))
```

## Pré-requisitos por método

{{PREREQUISITES}}

## Limpeza após execução

{{CLEANUP_INSTRUCTIONS}}

## Limitações de cobertura

{{COVERAGE_LIMITATIONS}}

## Observações

- Cada método retorna `.T.` (sucesso) ou `.F.` (falha)
- Log mínimo via `conout()` após cada execução
- Métodos são independentes — podem ser executados em qualquer ordem
- Setup de pré-requisitos é responsabilidade do executor do teste
- Arquivo gerado é TLPP puro (Protheus 12.1.2510+)

