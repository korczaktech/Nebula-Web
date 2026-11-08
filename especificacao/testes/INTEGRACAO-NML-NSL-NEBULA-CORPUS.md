# Corpus de integração — NML, NSL e Nebula

**Versão:** 0.1.0-draft  
**Estado:** cenários especificados; não executados  
**Contrato:** [CONTRATOS-NML-NSL-NEBULA.md](../CONTRATOS-NML-NSL-NEBULA.md)

## 1. Objetivo

Verificar a interoperabilidade entre frontends independentes sem fundir suas gramáticas. Os casos devem ser executados somente quando os frontends e contratos correspondentes existirem.

## 2. Casos positivos

| ID | Cenário | Resultado esperado |
|---|---|---|
| INT-P-001 | NML importa arquivo NSL válido | referência resolvida |
| INT-P-002 | NSL seleciona elemento NML conhecido | seletor validado |
| INT-P-003 | NSL seleciona classe declarada em NML | classe resolvida |
| INT-P-004 | NML declara componente importado | contrato do componente aplicado |
| INT-P-005 | NSL usa propriedade e valor compatíveis | tipo validado |
| INT-P-006 | NML e NSL usam mesma versão de contrato | integração aceita |
| INT-P-007 | expressão Nebula autorizada em propriedade NML | tipo e efeitos validados |
| INT-P-008 | função Nebula pura usada em valor NSL | retorno tipado validado |
| INT-P-009 | erro de NSL apontado a partir de import NML | origem do arquivo NSL preservada |
| INT-P-010 | compilação isolada NML | não exige parser NSL |
| INT-P-011 | compilação isolada NSL | não exige parser NML |
| INT-P-012 | source map atravessa compilação integrada | origem NML/NSL/Nebula recuperável |

## 3. Casos negativos

| ID | Cenário | Resultado esperado |
|---|---|---|
| INT-N-001 | NML referencia arquivo NSL inexistente | INT-0001 |
| INT-N-002 | NSL seleciona propriedade/estado inexistente | diagnóstico de resolução |
| INT-N-003 | expressão Nebula em contexto não autorizado | INT-0003 |
| INT-N-004 | expressão Nebula retorna tipo incompatível | erro de tipo na fronteira |
| INT-N-005 | contrato de componente incompatível | INT-0002 |
| INT-N-006 | versão NML/NSL incompatível | INT-0002 |
| INT-N-007 | ciclo de imports proibido | erro de dependência |
| INT-N-008 | seletor atravessa escopo isolado | erro de segurança |
| INT-N-009 | referência ambígua a componente | erro de resolução |
| INT-N-010 | runtime tenta expor syscall a documento comum | bloqueio de segurança |

## 4. Invariantes

1. ASTs NML e NSL permanecem tipos distintos.
2. Cada erro aponta para o arquivo e intervalo de origem correto.
3. Parser de uma linguagem nunca interpreta texto da outra.
4. Símbolos compartilhados têm identidade e versão estáveis.
5. Integração não amplia permissões do documento.
6. Mesmo conjunto de fontes/versões produz resultado determinístico.
7. Acesso especial ZenithOS só existe pelo adaptador Portunus no ZenithOS e com autorização do kernel.

## 5. Critério de aprovação

Todos os casos positivos e negativos devem passar em CI quando a implementação existir. A suíte deve incluir testes diferenciais, regressão, segurança e limites de recursos. Até lá, todos os casos permanecem especificados, não executados.
