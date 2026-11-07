# NSL — Gramática da Fase 3

**Estado:** gramática concreta inicial — rascunho não normativo  
**Versão:** 0.1.0-draft  
**Referência léxica e sintática:** `NSL-FASE-3-LEXICO-E-SINTAXE.md`

> Este arquivo separa a gramática EBNF do registro de decisões. A gramática é uma base executável para revisão e geração futura de testes, mas não deve ser tratada como contrato de parser até passar por revisão, corpus de conformidade e implementação de referência.

## 1. Notação

- EBNF: `=` definição; `|` alternativa; `[ ... ]` opcional; `{ ... }` repetição.
- Terminais aparecem entre aspas.
- Comentários explicativos aparecem fora das produções.
- `quebra` e `indentacao` são tokens virtuais produzidos pelo lexer/layout validator.
- A indentação é validada mesmo com delimitadores explícitos de bloco.

## 2. Arquivo e módulos

```ebnf
arquivo               = { quebra }, [ declaracao_modulo ], { quebra },
                        { declaracao_topo }, { quebra } ;

declaracao_modulo     = "modulo", nome_qualificado, bloco ;
declaracao_topo       = importacao
                      | exportacao
                      | declaracao_constante
                      | declaracao_variavel
                      | declaracao_funcao
                      | declaracao_tipo
                      | declaracao_estilo
                      | declaracao_tema
                      | declaracao_componente
                      | declaracao_propriedade
                      | declaracao_extensao ;

importacao            = "importar", caminho_modulo,
                        [ "como", identificador ], ";" ;
exportacao            = "exportar", identificador, ";" ;
caminho_modulo        = nome_qualificado | string ;
nome_qualificado      = identificador, { ".", identificador } ;
```

## 3. Blocos e declarações

```ebnf
bloco                 = "{", { quebra | declaracao_bloco }, "}" ;
declaracao_bloco      = importacao_local
                      | declaracao_constante
                      | declaracao_variavel
                      | declaracao_funcao
                      | declaracao_tipo
                      | declaracao_estilo
                      | declaracao_tema
                      | declaracao_componente
                      | declaracao_propriedade
                      | declaracao_estado
                      | declaracao_evento
                      | declaracao_token
                      | declaracao_animacao
                      | declaracao_transicao
                      | declaracao_consulta ;

importacao_local      = "importar", caminho_modulo,
                        [ "como", identificador ], ";" ;
declaracao_constante  = [ modificador_visibilidade ], "constante",
                        identificador, [ anotacao_tipo ],
                        "=", expressao, ";" ;
declaracao_variavel   = [ modificador_visibilidade ], "variavel",
                        identificador, [ anotacao_tipo ],
                        "=", expressao, ";" ;
modificador_visibilidade
                      = "publico" | "privado" | "protegido" ;
anotacao_tipo         = ":", tipo ;
declaracao_tipo       = "tipo", identificador, "=", tipo, ";" ;
declaracao_funcao     = [ modificador_visibilidade ], "funcao",
                        identificador, assinatura_funcao, bloco ;
assinatura_funcao    = "(", [ lista_parametros ], ")",
                        [ ":", tipo ] ;
lista_parametros      = parametro, { ",", parametro } ;
parametro             = [ "variavel" | "constante" ],
                        identificador, [ ":", tipo ],
                        [ "=", expressao ] ;
declaracao_componente = [ modificador_visibilidade ],
                        "componente", identificador,
                        [ assinatura_componente ], bloco ;
assinatura_componente = "(", [ lista_parametros ], ")" ;
declaracao_tema       = "tema", identificador, bloco ;
declaracao_estado     = "estado", identificador, [ anotacao_tipo ],
                        "=", expressao, ";" ;
declaracao_evento     = "evento", identificador,
                        [ assinatura_funcao ], ";" ;
declaracao_token      = "token", identificador, [ anotacao_tipo ],
                        "=", expressao, ";" ;
declaracao_propriedade
                      = "propriedade", identificador,
                        [ anotacao_tipo ], [ "=", expressao ], ";" ;
declaracao_extensao   = "estender", nome_qualificado, bloco ;
```

## 4. Declarações visuais

```ebnf
declaracao_estilo     = "estilo", "(", seletor, ")", bloco_estilo ;
bloco_estilo          = "{", { quebra | declaracao_visual }, "}" ;
declaracao_visual     = chamada_propriedade
                      | declaracao_estilo_aninhado
                      | declaracao_token
                      | declaracao_consulta
                      | declaracao_animacao
                      | declaracao_transicao ;
chamada_propriedade  = identificador_qualificado,
                        "(", [ lista_argumentos ], ")", ";" ;
declaracao_estilo_aninhado
                      = "estilo", "(", seletor, ")", bloco_estilo ;
declaracao_consulta  = "consulta", "(", expressao_condicao, ")", bloco ;
declaracao_animacao  = "animacao", identificador,
                        [ assinatura_funcao ], bloco ;
declaracao_transicao = "transicao", identificador,
                        [ assinatura_funcao ], bloco ;
```

Uma chamada de propriedade só é válida em um contexto visual autorizado e se o nome resolver para uma propriedade conhecida ou extensão importada. O parser pode reconhecer a forma sintática genérica; a análise semântica determina a categoria do símbolo.

## 5. Seletores tipados

```ebnf
seletor               = chamada_seletor
                      | grupo_seletores ;
grupo_seletores       = "seletor", ".", "grupo",
                        "(", chamada_seletor, ",",
                        chamada_seletor, { ",", chamada_seletor }, ")" ;
chamada_seletor       = "seletor", ".", nome_seletor,
                        "(", [ lista_argumentos ], ")" ;
nome_seletor          = "elemento"
                      | "componente"
                      | "classe"
                      | "id"
                      | "atributo"
                      | "estado"
                      | "slot"
                      | "descendente"
                      | "filho"
                      | "irmao"
                      | "irmaoPosterior"
                      | "parte"
                      | "contexto" ;
```

O significado e a aridade de cada seletor são definidos pelo catálogo semântico. Relações recebem seletores como argumentos; os argumentos são validados recursivamente.

## 6. Expressões

```ebnf
expressao             = atribuicao ;
atribuicao            = condicional,
                        [ operador_atribuicao, atribuicao ] ;
operador_atribuicao   = "=" | "+=" | "-=" | "*=" | "/=" ;
condicional           = pipeline,
                        [ "?", expressao, ":", expressao ] ;
pipeline              = coalescencia,
                        { "|>", coalescencia } ;
coalescencia          = logica_ou, [ "??", coalescencia ] ;
logica_ou             = logica_e, { "ou", logica_e } ;
logica_e              = igualdade, { "e", igualdade } ;
igualdade             = comparacao, { ( "==" | "!=" ), comparacao } ;
comparacao            = intervalo,
                        { ( "<" | "<=" | ">" | ">=" ), intervalo } ;
intervalo             = aditiva, [ "..", aditiva ] ;
aditiva               = multiplicativa,
                        { ( "+" | "-" ), multiplicativa } ;
multiplicativa        = unaria,
                        { ( "*" | "/" | "%" ), unaria } ;
unaria                = [ "+" | "-" | "nao" | "&" ], posfixa ;
posfixa               = primaria, { sufixo } ;
sufixo                = acesso
                      | acesso_opcional
                      | chamada
                      | indexacao ;
acesso                = ".", identificador ;
acesso_opcional       = "?.", identificador ;
chamada               = "(", [ lista_argumentos ], ")" ;
indexacao             = "[", expressao, "]" ;
primaria              = literal
                      | identificador_qualificado
                      | agrupamento
                      | lista
                      | funcao_lambda ;
agrupamento           = "(", expressao, ")" ;
lista                 = "[", [ expressao, { ",", expressao } ], "]" ;
funcao_lambda         = "(", [ lista_parametros ], ")", "=>", expressao ;
lista_argumentos      = argumento, { ",", argumento } ;
argumento             = [ identificador, "=" ], expressao ;
```

Comparações encadeadas não são permitidas implicitamente. Atribuição é associativa à direita. A associatividade dos demais operadores deverá ser fixada na tabela normativa de precedência.

## 7. Tipos

```ebnf
tipo                  = tipo_uniao ;
tipo_uniao            = tipo_intersecao, { "|", tipo_intersecao } ;
tipo_intersecao       = tipo_posfixo, { "&", tipo_posfixo } ;
tipo_posfixo          = tipo_atomico, [ "?", { sufixo_tipo } ] ;
tipo_atomico          = nome_qualificado
                      | tipo_funcao
                      | tipo_lista
                      | tipo_tupla
                      | tipo_generico ;
tipo_funcao           = "funcao", "(", [ lista_tipos ], ")", ":", tipo ;
tipo_lista            = "[", tipo, "]" ;
tipo_tupla            = "(", tipo, { ",", tipo }, ")" ;
tipo_generico         = nome_qualificado, "<", lista_tipos, ">" ;
lista_tipos           = tipo, { ",", tipo } ;
sufixo_tipo           = "[", "]" ;
```

A grafia de tipos primitivos, tipos numéricos, tipos de medida, unidades, tipos de cor e tipos de plataforma será definida no catálogo de tipos. O parser não presume que todo identificador seja tipo válido.

## 8. Literais, strings e comentários

```ebnf
literal               = numero
                      | string
                      | "verdadeiro"
                      | "falso"
                      | "nulo"
                      | "ausente" ;
string                = string_simples
                      | string_dupla
                      | string_multilinha ;
numero                = inteiro | decimal ;
inteiro               = decimal_inteiro
                      | binario
                      | octal
                      | hexadecimal ;
decimal               = digitos, ".", digitos, [ expoente ]
                      | digitos, expoente ;
expoente              = ( "e" | "E" ), [ "+" | "-" ], digitos ;
binario               = "0b", digitos_binarios ;
octal                 = "0o", digitos_octais ;
hexadecimal           = "0x", digitos_hexadecimais ;
comentario            = comentario_linha | comentario_bloco
                      | comentario_documentacao ;
comentario_linha      = "//", { caractere_sem_quebra } ;
comentario_bloco      = "/*", { conteudo_bloco }, "*/" ;
comentario_documentacao
                      = "///", { caractere_sem_quebra } ;
```

O reconhecimento detalhado de escapes, interpolação, strings cruas, separadores numéricos e comentários aninhados pertence à especificação léxica. O lexer deverá produzir tokens de string segmentados para interpolação, sem misturar texto literal e expressão.

## 9. Layout e indentação

O lexer/layout validator emite tokens virtuais de quebra e nível de indentação. As regras são:
- indentação de bloco é constante no arquivo;
- não misturar tabulações e espaços no mesmo nível;
- chave de fechamento alinhada à declaração que abriu o bloco;
- a quebra de linha não encerra chamada ou expressão quando o parser espera continuação;
- bloco vazio é permitido apenas onde a produção e a semântica autorizarem;
- declarações simples exigem `;`;
- bloco fecha exclusivamente com `}`.

## 10. Pontos ainda não fechados

Esta gramática ainda não especifica integralmente:
- todas as declarações de temas, componentes, slots, variantes e estados;
- sintaxe de metadados e diretivas de compilação;
- forma completa de generics, constraints, overloads e efeitos;
- escapes e interpolação em todas as categorias de string;
- aridade e assinatura normativa de todos os seletores e funções;
- regras completas de precedência e associatividade;
- recovery grammar e sincronização do parser;
- propriedades e tipos de unidade nativos.

Esses itens são trabalho técnico da especificação, não perguntas abertas de preferência. A gramática só será promovida a normativa depois da revisão e dos testes de conformidade.
