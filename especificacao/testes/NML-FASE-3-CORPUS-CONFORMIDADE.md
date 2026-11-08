# NML — Corpus de conformidade da Fase 3

**Versão:** 0.1.0-draft  
**Estado:** casos especificados; ainda não executados  
**Referências:** gramática NML, léxico NML e modelo semântico NML.

> Este documento especifica resultados esperados. Não contém implementação de testes e não representa execução.

## 1. Contrato

Cada caso registra entrada, etapa, aceitação/rejeição, código de diagnóstico, intervalo de origem e forma essencial da AST. Casos inválidos não podem produzir AST semanticamente válida.

## 2. Casos positivos

| ID | Recurso | Resultado esperado |
|---|---|---|
| NML-LEX-P-001 | UTF-8 válido com português | aceitar |
| NML-LEX-P-002 | identificador camelCase | aceitar |
| NML-LEX-P-003 | identificador Unicode XID | aceitar e comparar em NFC |
| NML-LEX-P-004 | strings simples/duplas | tokens string |
| NML-LEX-P-005 | string multilinha | token multilinha |
| NML-LEX-P-006 | inteiros em bases suportadas | tokens numéricos |
| NML-LEX-P-007 | decimal científico | token decimal |
| NML-LEX-P-008 | comentários linha/bloco | ignorar sem alterar AST |
| NML-SYN-P-001 | arquivo vazio | unidade documental vazia |
| NML-SYN-P-002 | somente cabecalho | AST com região cabecalho |
| NML-SYN-P-003 | somente pagina | AST com região pagina |
| NML-SYN-P-004 | somente rodape | AST com região rodape |
| NML-SYN-P-005 | três regiões juntas | três regiões distintas |
| NML-SYN-P-006 | regiões em ordem diferente | aceitar e preservar origem |
| NML-SYN-P-007 | div aninhada em div | árvore aninhada |
| NML-SYN-P-008 | propriedade texto em t1 | TextValueNode |
| NML-SYN-P-009 | lista e objeto | nós tipados |
| NML-SYN-P-010 | metadados opcional | região auxiliar |
| NML-SYN-P-011 | referência NSL externa | StyleReferenceNode |
| NML-SEM-P-001 | elemento com propriedades válidas | AST válida |
| NML-SEM-P-002 | nulo em propriedade anulável | aceitar |
| NML-SEM-P-003 | componente importado compatível | resolver componente |
| NML-SEM-P-004 | texto em elemento textual | validar conteúdo |

## 3. Casos negativos

| ID | Recurso | Resultado esperado |
|---|---|---|
| NML-LEX-N-001 | UTF-8 inválido | erro lexical com offset |
| NML-LEX-N-002 | identificador iniciado por dígito | rejeitar |
| NML-LEX-N-003 | string não terminada | erro com posição inicial |
| NML-LEX-N-004 | escape inválido | erro lexical |
| NML-LEX-N-005 | controle em identificador | rejeitar |
| NML-SYN-N-001 | propriedade sem dois-pontos | erro sintático |
| NML-SYN-N-002 | bloco sem fechamento | erro e recuperação |
| NML-SYN-N-003 | chave extra | erro sintático |
| NML-SYN-N-004 | texto livre no bloco | erro sintático |
| NML-SYN-N-005 | propriedade sem valor | erro sintático |
| NML-SYN-N-006 | região principal duplicada | erro semântico |
| NML-SYN-N-007 | região auxiliar duplicada | erro semântico |
| NML-SYN-N-008 | propriedade duplicada não acumulável | erro semântico |
| NML-SYN-N-009 | indentação divergente | erro de layout |
| NML-SYN-N-010 | tabulação em indentação | erro de layout |
| NML-SEM-N-001 | elemento desconhecido | erro de resolução |
| NML-SEM-N-002 | propriedade desconhecida | erro de resolução |
| NML-SEM-N-003 | tipo incompatível | erro de tipo |
| NML-SEM-N-004 | filho proibido pelo pai | erro estrutural |
| NML-SEM-N-005 | texto e filhos em conteúdo exclusivo | erro semântico |
| NML-SEM-N-006 | referência NSL inexistente | erro de dependência |
| NML-SEM-N-007 | ciclo de referência proibido | erro de dependência |

## 4. Recuperação

Testar erro no início, no meio de propriedades, dentro de elemento aninhado, antes de região seguinte e múltiplos erros independentes. Recuperação preserva nós válidos posteriores e insere ErrorNode para trechos inválidos.

## 5. Segurança, limites e determinismo

Testar arquivo vazio/grande, profundidade máxima, aninhamento extremo, comentário longo/não terminado, escapes malformados, referência externa inválida, Unicode confundível e fuzzing. Mesma entrada, versão e dependências produzem AST e diagnósticos idênticos. Fuzzing não pode causar crash, panic, overflow não tratado ou consumo ilimitado.

## 6. Cobertura

Toda produção EBNF tem caso positivo e toda regra de rejeição tem caso negativo. Todos os casos permanecem pendentes até execução por frontend de referência. Nenhum resultado é presumido.
