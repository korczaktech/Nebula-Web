# NSL — Corpus de conformidade da Fase 3

**Estado:** corpus especificado; execução depende de lexer/parser de referência  
**Versão:** 0.2.0-draft  
**Gramática-base:** `GRAMATICA-NSL-FASE-3.md`  
**Regras léxicas:** `NSL-FASE-3-LEXICO-E-SINTAXE.md`

 > Os exemplos deste documento definem resultados esperados para a sintaxe direta 0.2.0 e extensões documentadas. Ainda não são resultados de testes executados. Um caso só será aprovado após execução pelo lexer/parser real.

## 1. Contrato do corpus

Cada caso deve verificar:
- aceitação ou rejeição do arquivo;
- categoria e código estável do diagnóstico;
- intervalo de origem (linha, coluna e offsets);
- recuperação do parser, quando aplicável;
- AST produzida para entradas válidas;
- ausência de AST semanticamente válida para entradas rejeitadas;
- estabilidade entre execuções e plataformas suportadas.

O corpus deve ser executado em modo unitário, integração, regressão e fuzzing. Cada regra gramatical normativa deve possuir ao menos um caso positivo e um negativo, salvo quando a rejeição não fizer sentido para a produção.

## 2. Casos lexicais positivos

| ID | Recurso | Entrada mínima | Resultado esperado |
|---|---|---|---|
| LEX-P-001 | UTF-8 | Arquivo com texto português e símbolos Unicode válidos | Aceitar |
| LEX-P-002 | Identificador ASCII | `botaoPrincipal` | Aceitar |
| LEX-P-003 | Identificador Unicode | Identificador com letra Unicode XID válida | Aceitar e normalizar NFC |
| LEX-P-004 | Inteiro decimal | `120` | Token inteiro |
| LEX-P-005 | Hexadecimal | `0x7f` | Token inteiro hexadecimal |
| LEX-P-006 | Binário | `0b1010` | Token inteiro binário |
| LEX-P-007 | Decimal | `12.75` | Token decimal |
| LEX-P-008 | Expoente | `1.25e-3` | Token decimal científico |
| LEX-P-009 | String simples | `'texto'` | Token string |
| LEX-P-010 | String dupla | `"texto"` | Token string |
| LEX-P-011 | String multilinha | String delimitada pela forma multilinha definida no léxico | Token string multilinha |
| LEX-P-012 | Comentário de linha | `// comentário` | Ignorar sem alterar tokens seguintes |
| LEX-P-013 | Comentário de bloco | `/* comentário */` | Ignorar |
| LEX-P-014 | Comentário aninhado | Blocos de comentário aninhados | Ignorar com balanceamento correto |
| LEX-P-015 | Palavras lógicas | `e ou nao` em expressão | Tokens de operador lógico |

## 3. Casos léxicos negativos

| ID | Recurso | Entrada mínima | Resultado esperado |
|---|---|---|---|
| LEX-N-001 | UTF-8 inválido | Sequência de bytes inválida | Erro léxico com offset |
| LEX-N-002 | Identificador iniciado por dígito | `9botao` | Rejeitar como identificador |
| LEX-N-003 | Identificador com controle | Identificador contendo caractere de controle | Rejeitar |
| LEX-N-004 | Confusable suspeito | Identificadores visualmente confundíveis no mesmo escopo | Diagnóstico de segurança |
| LEX-N-005 | Separador numérico inicial | `_12` como literal numérico | Rejeitar |
| LEX-N-006 | Separador duplicado | `1__2` | Rejeitar |
| LEX-N-007 | Expoente incompleto | `1e+` | Rejeitar |
| LEX-N-008 | Base sem dígitos | `0x` | Rejeitar |
| LEX-N-009 | String não terminada | Aspas sem fechamento | Rejeitar com posição de abertura |
| LEX-N-010 | Escape inválido | Escape Unicode fora do formato | Rejeitar |
| LEX-N-011 | Comentário não terminado | `/*` sem fechamento | Rejeitar |
| LEX-N-012 | Delimitador desconhecido | Caractere não previsto na gramática | Rejeitar |

## 4. Casos sintáticos positivos

| ID | Recurso | Entrada | Resultado esperado |
|---|---|---|---|
| SYN-P-001 | Arquivo vazio | Arquivo vazio ou apenas comentários | Aceitar como unidade vazia, se permitido pelo perfil |
| SYN-P-002 | Módulo | `modulo exemplo { }` | AST de módulo |
| SYN-P-003 | Importação | `importar tema.claro como claro;` | AST de importação |
| SYN-P-004 | Constante | `constante espacamento = 8;` | AST de constante |
| SYN-P-005 | Variável | `variavel largura = 320;` | AST de variável |
| SYN-P-006 | Função | `funcao calcular(x: Numero): Numero { retorne x; }` | AST de função |
| SYN-P-007 | Estilo | `estilo(seletor.elemento("button")) { cor(cor.hex("#fff")); }` | AST de regra e declaração visual |
| SYN-P-008 | Seletor de classe | `seletor.classe("botaoPrincipal")` | AST de seletor de classe |
| SYN-P-009 | Seletor composto | `seletor.descendente(seletor.elemento("main"), seletor.classe("titulo"))` | AST relacional |
| SYN-P-010 | Lista | `[1, 2, 3]` | AST de lista |
| SYN-P-011 | Argumento nomeado | `funcaoX(valor = 10)` | AST de chamada com argumento nomeado |
| SYN-P-012 | Tipo composto | Tipo de união e tipo genérico conforme gramática | AST tipada |
| SYN-P-013 | Expressão lógica | `verdadeiro e nao falso` | AST lógica |
| SYN-P-014 | Coalescência | `valor ?? padrao` | AST de coalescência |
| SYN-P-015 | Acesso opcional | `objeto?.campo` | AST de acesso opcional |
| SYN-P-016 | Pipeline | `entrada |> transformar` | AST de composição |
| SYN-P-017 | Bloco aninhado | Blocos com chaves e indentação consistente | Aceitar |
| SYN-P-018 | Comentário interno | Comentários entre declarações | Aceitar sem alterar AST semântica |

## 5. Casos sintáticos negativos

| ID | Recurso | Entrada | Resultado esperado |
|---|---|---|---|
| SYN-N-001 | Palavra reservada como nome | `constante se = 1;` | Rejeitar |
| SYN-N-002 | Delimitador ausente | Chamada sem `)` | Rejeitar e recuperar |
| SYN-N-003 | Bloco sem fechamento | `modulo exemplo { constante x = 1;` | Rejeitar |
| SYN-N-004 | Chave extra | `modulo exemplo { } }` | Rejeitar |
| SYN-N-005 | Terminador ausente | Declaração simples sem `;` | Rejeitar |
| SYN-N-006 | Argumentos duplicados | Dois argumentos nomeados com o mesmo nome | Rejeitar semanticamente |
| SYN-N-007 | Vírgula final proibida | Lista com vírgula final, se não autorizada pelo perfil | Rejeitar |
| SYN-N-008 | Operador sem operando | `valor + ;` | Rejeitar |
| SYN-N-009 | Comparação encadeada | `a < b < c` | Rejeitar ou exigir agrupamento explícito |
| SYN-N-010 | Atribuição a constante | Atribuição para símbolo constante | Rejeitar semanticamente |
| SYN-N-011 | Propriedade fora de contexto | `cor(cor.hex("#fff"));` no topo do módulo | Rejeitar semanticamente |
| SYN-N-012 | Propriedade sem terminador | Chamada de propriedade sem `;` | Rejeitar |
| SYN-N-013 | Indentação divergente | Linhas irmãs com largura inconsistente | Rejeitar |
| SYN-N-014 | Mistura de tabs e espaços | Mistura no mesmo nível | Rejeitar |
| SYN-N-015 | Fechamento desalinhado | Chave de fechamento em coluna incompatível | Rejeitar |
| SYN-N-016 | Seletor vazio | `estilo() { }` | Rejeitar |
| SYN-N-017 | Tipo ausente | Anotação `:` sem tipo | Rejeitar |
| SYN-N-018 | Declaração incompleta | `constante x = ;` | Rejeitar e recuperar |

## 6. Casos semânticos

| ID | Recurso | Condição | Resultado esperado |
|---|---|---|---|
| SEM-001 | Símbolo não declarado | Referência a nome inexistente | Erro de resolução |
| SEM-002 | Nome duplicado | Declaração conflitante no mesmo escopo | Erro de duplicidade |
| SEM-003 | Tipo incompatível | Valor incompatível com anotação | Erro de tipo |
| SEM-004 | Unidade incompatível | Operação dimensional inválida | Erro dimensional |
| SEM-005 | Propriedade desconhecida | Propriedade não registrada/importada | Erro ou diagnóstico conforme extensão |
| SEM-006 | Valor fora do domínio | Valor proibido pela propriedade | Erro de validação |
| SEM-007 | Seletor impossível | Seletor incompatível com o contexto de componentes | Diagnóstico contextual |
| SEM-008 | Importação circular | Ciclo não permitido pela política de módulos | Erro de dependência |
| SEM-009 | Acesso privado | Referência externa a símbolo privado | Erro de visibilidade |
| SEM-010 | Alias ambíguo | Dois imports resolvem o mesmo nome sem qualificação | Erro de ambiguidade |
| SEM-011 | Função sem retorno | Caminho obrigatório sem retorno | Diagnóstico de fluxo |
| SEM-012 | Operador inválido | Operador sem implementação para os tipos envolvidos | Erro de operador |

## 7. Testes de recuperação

O parser deve ser testado com pelo menos:
1. erro no início do arquivo;
2. erro no meio de uma lista de declarações;
3. erro dentro de argumentos;
4. erro em expressão aninhada;
5. erro em bloco de estilo;
6. erro em declaração de função;
7. dois erros independentes no mesmo arquivo;
8. delimitador ausente antes de uma declaração válida seguinte.

A recuperação deve sincronizar em delimitadores e inícios de declaração sem consumir o próximo bloco válido indevidamente. A AST recuperada deve marcar nós sintéticos e não pode ser confundida com AST livre de erros.

## 8. Testes de estabilidade e segurança

- Mesma entrada e mesma versão de compilador produzem os mesmos tokens, AST e diagnósticos.
- NFC e sequências canonicamente equivalentes são resolvidas conforme a política Unicode.
- Confusables geram diagnósticos reproduzíveis.
- Arquivos muito grandes, comentários profundos, aninhamento extremo e cadeias de operadores respeitam limites de recursos.
- Fuzzing não pode causar panic, crash, overflow não tratado ou consumo ilimitado.
- Parser incremental e parser completo devem produzir AST semanticamente equivalentes.
- Formatação não altera a semântica do programa.

## 9. Critérios de saída da Fase 3

A especificação da Fase 3 poderá ser considerada formalizada quando:
1. todas as palavras reservadas e tokens tiverem inventário fechado e versionado;
2. todas as produções gramaticais tiverem semântica e precedência documentadas;
3. não houver referências EBNF sem definição ou terminais sem regra lexical;
4. o corpus cobrir todas as produções e restrições relevantes;
5. os exemplos positivos e negativos forem revisados;
6. a gramática for aprovada como normativa;
7. o lexer/parser de referência passar integralmente no corpus;
8. as limitações restantes forem registradas como itens de implementação, não como ambiguidades da especificação.

**Estado atual:** o corpus foi especificado, mas não executado. A conclusão da Fase 3 depende da gramática final e da implementação de referência.


---

## 10. Adendo de conformidade — sintaxe direta 0.2.0

A partir da revisão 0.2.0, os casos básicos obrigatórios incluem:

| ID | Entrada | Resultado esperado |
|---|---|---|
| NSL-DIR-P-001 | `.nome: { cor: "#fff"; }` | seletor de classe e declaração |
| NSL-DIR-P-002 | `nome: { cor: "#fff"; }` | seletor de elemento e declaração |
| NSL-DIR-P-003 | `.nome { ... }` | rejeitar: dois-pontos obrigatório |
| NSL-DIR-N-001 | `estilo(seletor.elemento("nome")) { ... }` | rejeitar no perfil canônico 0.2.0 |
| NSL-DIR-N-002 | declaração sem `;` | rejeitar |
| NSL-DIR-N-003 | nome de propriedade desconhecido | erro semântico |

Os exemplos antigos de sintaxe funcional permanecem somente como histórico de proposta e não podem ser usados como casos positivos da gramática 0.2.0.


## 11. Adendo normativo final — NSL 0.2.0

Este adendo substitui todos os casos positivos antigos que usam a forma funcional de regra ou propriedade. A entrada funcional antiga é sempre negativa no perfil 0.2.0.

### 11.1 Casos positivos obrigatórios

| ID | Entrada | Resultado esperado |
|---|---|---|
| NSL-DIR-P-001 | `.nome: { cor: "#fff"; }` | regra de classe |
| NSL-DIR-P-002 | `botao: { cor: "#fff"; }` | regra de elemento |
| NSL-DIR-P-003 | `#acao: { opacidade: 1; }` | regra de id |
| NSL-DIR-P-004 | `main .titulo: { tamanhoFonte: 16px; }` | relação descendente |
| NSL-DIR-P-005 | `main > .titulo: { tamanhoFonte: 16px; }` | filho direto |
| NSL-DIR-P-006 | `.a, .b: { cor: "red"; }` | grupo de seletores |
| NSL-DIR-P-007 | `.x[ativo="sim"]: { cor: "red"; }` | atributo |
| NSL-DIR-P-008 | `.x:hover: { cor: "red"; }` | estado |
| NSL-DIR-P-009 | `.x::parte: { cor: "red"; }` | parte |
| NSL-DIR-P-010 | `.x: { largura: 10px; duracao: 200ms; angulo: 1turn; }` | unidades tipadas |
| NSL-DIR-P-011 | `.x: { fundo: $tema.corDestaque; }` | referência a token |
| NSL-DIR-P-012 | `.x: { largura: soma(2px, 3px); }` | chamada de função catalogada |

### 11.2 Casos negativos obrigatórios

| ID | Entrada/condição | Resultado esperado |
|---|---|---|
| NSL-DIR-N-001 | `estilo(seletor.elemento("x")) { ... }` | rejeitar sintaxe funcional histórica |
| NSL-DIR-N-002 | `cor("#fff");` como declaração | rejeitar chamada funcional de propriedade |
| NSL-DIR-N-003 | `.x { cor: "red"; }` | rejeitar: dois-pontos após seletor obrigatório |
| NSL-DIR-N-004 | declaração `cor: "red"` sem `;` | erro sintático |
| NSL-DIR-N-005 | `.x: { largura: 2foo; }` | erro lexical de unidade desconhecida |
| NSL-DIR-N-006 | `.x: { largura: 2s; }` | erro semântico de dimensão |
| NSL-DIR-N-007 | `.x: { cor: !important; }` | rejeitar marcador de prioridade não definido |
| NSL-DIR-N-008 | seletor incompleto ou combinador sem operando | erro sintático |
| NSL-DIR-N-009 | propriedade duplicada não acumulável | erro semântico |
| NSL-DIR-N-010 | chamada de função não catalogada | erro de resolução |

### 11.3 Espaços e desambiguação

Casos específicos devem verificar espaço descendente entre seletores, espaços de formatação em valores, quebra dentro de delimitadores, comentários entre tokens, e que espaço antes/depois de dois-pontos ou ponto e vírgula nunca cria combinador. O mesmo arquivo deve produzir a mesma sequência de tokens e AST em execuções repetidas.

**Estado:** corpus alinhado documentalmente à sintaxe direta; não executado.
