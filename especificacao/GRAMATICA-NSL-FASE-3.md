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
                      | declaracao_consulta
                      | comando_condicional
                      | comando_escolha
                      | comando_para
                      | comando_enquanto
                      | comando_retorno
                      | comando_tentar
                      | comando_lancar
                      | comando_controle ;

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
declaracao_funcao     = [ modificador_visibilidade ], [ "assinc" ], "funcao",
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
chamada_propriedade  = nome_qualificado,
                        "(", [ lista_argumentos ], ")", ";" ;
declaracao_estilo_aninhado
                      = "estilo", "(", seletor, ")", bloco_estilo ;
declaracao_consulta  = "consulta", "(", expressao, ")", bloco ;

comando_condicional   = "se", "(", expressao, ")", bloco,
                        [ "senao", ( bloco | comando_condicional ) ] ;
comando_escolha       = "escolha", "(", expressao, ")", "{",
                        { quebra | ramo_caso }, [ ramo_padrao ], "}" ;
ramo_caso             = "caso", lista_expressoes, ":", bloco ;
ramo_padrao           = "senao", ":", bloco ;
lista_expressoes      = expressao, { ",", expressao } ;
comando_para          = "para", "(", identificador, "em", expressao, ")", bloco ;
comando_enquanto      = "enquanto", "(", expressao, ")", bloco ;
comando_retorno       = "retorne", [ expressao ], ";" ;
comando_tentar        = "tente", bloco,
                        { "capturar", "(", [ parametro ], ")", bloco },
                        [ "finalmente", bloco ] ;
comando_lancar        = "lancar", expressao, ";" ;
comando_controle      = "interromper", ";" | "continuar", ";" ;
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
unaria                = [ "+" | "-" | "nao" | "&" | "aguardar" ], posfixa ;
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
                      | nome_qualificado
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

## 10. Tokens lexicais

Os itens abaixo são reconhecidos pelo lexer antes da análise sintática. Os nomes em maiúsculas são classes de tokens internas, não texto digitado no arquivo.

```ebnf
identificador         = caractere_inicio, { caractere_continuacao } ;
caractere_inicio      = "_" | UNICODE_XID_START ;
caractere_continuacao = "_" | UNICODE_XID_CONTINUE ;
quebra                = TOKEN_QUEBRA ;
string_simples        = TOKEN_STRING_SIMPLES ;
string_dupla          = TOKEN_STRING_DUPLA ;
string_multilinha     = TOKEN_STRING_MULTILINHA ;
comentario_linha      = TOKEN_COMENTARIO_LINHA ;
comentario_bloco      = TOKEN_COMENTARIO_BLOCO ;
comentario_documentacao
                      = TOKEN_COMENTARIO_DOCUMENTACAO ;
digitos               = digito, { [ "_" ], digito } ;
digitos_binarios      = digito_binario, { [ "_" ], digito_binario } ;
digitos_octais        = digito_octal, { [ "_" ], digito_octal } ;
digitos_hexadecimais  = digito_hexadecimal,
                        { [ "_" ], digito_hexadecimal } ;
digito                = "0" | "1" | "2" | "3" | "4"
                      | "5" | "6" | "7" | "8" | "9" ;
digito_binario        = "0" | "1" ;
digito_octal          = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" ;
digito_hexadecimal    = digito | "a" | "b" | "c" | "d" | "e" | "f"
                      | "A" | "B" | "C" | "D" | "E" | "F" ;
decimal_inteiro       = digitos ;
```

Contrato lexical:
- `UNICODE_XID_START` e `UNICODE_XID_CONTINUE` representam as propriedades Unicode correspondentes na versão fixada pela versão NSL. Controles, não-caracteres e caracteres proibidos pela política de segurança são excluídos.
- Identificadores são sensíveis a maiúsculas/minúsculas e comparados após normalização NFC; a grafia original é preservada para diagnósticos e source maps.
- `TOKEN_STRING_SIMPLES`, `TOKEN_STRING_DUPLA` e `TOKEN_STRING_MULTILINHA` representam strings já balanceadas pelo lexer. Aspas simples e duplas aceitam escapes; strings multilinha aceitam quebras de linha.
- Interpolação produz segmentos alternados de texto e expressão, preservando origem e tipo de cada segmento.
- `TOKEN_COMENTARIO_BLOCO` reconhece comentários aninhados e limita profundidade para evitar consumo ilimitado.
- `TOKEN_QUEBRA` preserva offsets, número da linha e indentação original; o validador de layout produz também níveis virtuais de indentação.
- Separadores numéricos não podem aparecer no início/fim, em duplicidade ou imediatamente antes/depois do ponto decimal ou expoente.
- Literais numéricos são validados quanto a base, precisão, overflow e unidade durante as etapas léxica e semântica apropriadas.

## 11. Pontos ainda não fechados

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


---

## 12. Revisão de sintaxe 0.2.0 — seletores diretos e declarações por dois-pontos

**Esta seção é a revisão autoritativa da sintaxe de autoria.** Em caso de conflito, ela substitui as produções das seções 4 e 5 desta versão 0.1.0. A sintaxe básica aprovada é .nome para classe e nome para elemento. A forma funcional antiga estilo(seletor...) não é a forma canônica de autoria.

### 12.1 Regras visuais

```ebnf
regra_estilo       = grupo_seletores, ":", bloco_estilo ;
bloco_estilo       = "{", { QUEBRA | declaracao_visual }, "}" ;
declaracao_visual  = declaracao_valor | regra_estilo
                   | declaracao_token | declaracao_consulta ;
declaracao_valor   = IDENT, ":", valor_estilo, ";" ;
```

Exemplo:

```nsl
.nome: {
    cor: "#ffffff";
    tamanhoFonte: 16px;
}
```

### 12.2 Seletores diretos

```ebnf
seletor            = seletor_simples, { combinador, seletor_simples } ;
seletor_simples    = IDENT, { modificador_seletor }
                   | ".", IDENT, { modificador_seletor }
                   | "#", IDENT, { modificador_seletor }
                   | "*" ;
modificador_seletor = ".", IDENT | "#", IDENT
                    | seletor_atributo | seletor_estado
                    | seletor_parte ;
seletor_atributo   = "[", IDENT, [ operador_atributo, valor_estilo ], "]" ;
operador_atributo  = "=" | "!=" | "^=" | "$=" | "*=" | "~=" | "|=" ;
seletor_estado     = ":", IDENT, [ "(", [ lista_argumentos ], ")" ] ;
seletor_parte      = "::", IDENT ;
combinador         = ESPACO | ">" | "+" | "~" ;
grupo_seletores    = seletor, { ",", seletor } ;
medida             = NUMERO, UNIDADE ;
NUMERO             = TOKEN_NUMERO ;
UNIDADE            = TOKEN_UNIDADE ;
ESPACO             = TOKEN_ESPACO_COMBINADOR ;
cor                = literal_cor | chamada_valor ;
literal_cor        = STRING ;
referencia_token   = "$", nome_qualificado ;
lista_valores      = "[", [ valor_estilo, { ",", valor_estilo } ], "]" ;
```

### 12.3 Valores

```ebnf
valor_estilo       = literal | medida | referencia_token
                   | lista_valores | chamada_valor | expressao ;
chamada_valor      = nome_qualificado, "(", [ lista_argumentos ], ")" ;
```

A análise semântica valida tipos, dimensões, funções, aridade e domínio da propriedade. .nome significa classe; nome significa elemento. A extensão #nome representa id e obedece à unicidade do escopo.

### 12.4 Regras de precedência e layout

- Acesso/chamada/indexação; unários; multiplicação; adição; comparação; igualdade; e; ou; condicional; atribuição.
- Atribuição é associativa à direita; comparações encadeadas são rejeitadas.
- Blocos usam chaves e indentação obrigatória de quatro espaços.
- Tabs não são permitidos na indentação.
- Declarações de valor exigem ponto e vírgula; regras de seletor não recebem ponto e vírgula após o bloco.
- A chave de fechamento alinha-se à declaração que abriu o bloco.

### 12.5 Compatibilidade

A revisão 0.2.0 substitui as produções de regra visual e seletor da versão 0.1.0. Exemplos antigos com chamadas funcionais de seletor são históricos e não representam a sintaxe básica aprovada. Esta revisão continua draft até revisão e execução do corpus por implementação de referência.


## 13. Gramática normativa consolidada 0.2.0

Esta seção substitui as produções funcionais das seções anteriores. O perfil canônico não aceita `estilo(seletor...)` nem chamadas funcionais de propriedades.

~~~ebnf
arquivo             = { QUEBRA }, [ modulo ], { declaracao_topo | regra_estilo | QUEBRA }, EOF ;
modulo              = "modulo", nome_qualificado, bloco ;
declaracao_topo     = importacao | exportacao | declaracao_constante | declaracao_variavel
                    | declaracao_funcao | declaracao_tipo | declaracao_tema | declaracao_componente ;
importacao          = "importar", nome_qualificado, [ "como", IDENT ], ";" ;
exportacao          = "exportar", IDENT, ";" ;
regra_estilo        = grupo_seletores, ":", bloco_estilo ;
grupo_seletores     = seletor, { ",", seletor } ;
seletor             = seletor_simples, { combinador, seletor_simples } ;
seletor_simples     = [ tipo_elemento ], { modificador } | "*" ;
tipo_elemento       = IDENT ;
modificador         = ".", IDENT | "#", IDENT | seletor_atributo | seletor_estado | seletor_parte ;
seletor_atributo    = "[", IDENT, [ operador_atributo, literal ], "]" ;
operador_atributo   = "=" | "!=" | "^=" | "$=" | "*=" | "~=" | "|=" ;
seletor_estado      = ":", IDENT, [ "(", [ lista_argumentos ], ")" ] ;
seletor_parte       = "::", IDENT ;
combinador          = ESPACO_SELETOR | ">" | "+" | "~" ;
bloco_estilo        = "{", { QUEBRA | declaracao_visual }, "}" ;
declaracao_visual   = declaracao_valor | regra_estilo | declaracao_token | declaracao_consulta ;
declaracao_valor    = IDENT, ":", valor_estilo, ";" ;
valor_estilo        = literal | medida | referencia_token | lista_valores | chamada_valor | expressao ;
medida              = NUMERO, UNIDADE ;
referencia_token    = "$", nome_qualificado ;
lista_valores       = "[", [ valor_estilo, { ",", valor_estilo }, [ "," ] ], "]" ;
chamada_valor       = nome_qualificado, "(", [ lista_argumentos ], ")" ;
expressao           = termo, { operador_aritmetico, termo } ;
termo               = literal | medida | referencia_token | chamada_valor | "(", expressao, ")" ;
operador_aritmetico = "+" | "-" | "*" | "/" ;
literal             = STRING | NUMERO | "verdadeiro" | "falso" | "nulo" ;
lista_argumentos    = argumento, { ",", argumento } ;
argumento           = [ IDENT, "=" ], valor_estilo ;
nome_qualificado    = IDENT, { ".", IDENT } ;
~~~

### 13.1 Regras de desambiguação

1. Seletor de tipo é IDENT no início de seletor; classe exige ponto, id exige hash, universal é asterisco.
2. Um espaço entre dois seletores completos é combinador descendente. Espaço em torno de operador, dois-pontos, valor ou dentro de bloco é trivia.
3. A regra começa no contexto de seletor; uma declaração começa somente dentro de bloco de estilo e tem forma IDENT dois-pontos valor ponto-e-vírgula.
4. Chamadas de valor não são declarações de propriedade: só aparecem como valor e são resolvidas semanticamente.
5. Unidades são reconhecidas por catálogo fechado da versão; não são sufixos livres.
6. `!important` não pertence ao núcleo 0.2.0.

### 13.2 Restrições semânticas

Seletor, propriedade, função de valor, unidade e token devem resolver em catálogo/import. Propriedades duplicadas são erro, salvo política acumulável declarada. Valores devem satisfazer tipo, dimensão e domínio. Seletor sem correspondência é aviso por padrão; perfil estrito pode elevar a erro. Escopos isolados não são atravessados sem exportação explícita.

### 13.3 Rejeições obrigatórias

A forma `estilo(seletor.elemento("x")) { ... }`, chamadas `cor("#fff");`, regra sem dois-pontos, declaração sem ponto e vírgula, unidade desconhecida, valor sem tipo válido, seletor incompleto e marcador `!important` são rejeitados no perfil 0.2.0.


## 14. EBNF fechada do núcleo 0.2.0

Esta seção substitui a seção 13 anterior e todas as produções legadas incompatíveis. Cada não terminal utilizado abaixo está definido neste bloco ou é token explicitamente definido na especificação léxica 0.2.0.

~~~ebnf
arquivo              = { QUEBRA }, [ modulo ], { item_raiz }, EOF ;
item_raiz            = declaracao_topo | regra_estilo | QUEBRA ;
modulo               = "modulo", nome_qualificado, bloco ;
declaracao_topo      = importacao | exportacao | declaracao_constante
                     | declaracao_variavel | declaracao_funcao
                     | declaracao_tipo | declaracao_tema
                     | declaracao_componente ;
importacao           = "importar", nome_qualificado, [ "como", IDENT ], ";" ;
exportacao           = "exportar", IDENT, ";" ;
declaracao_constante = [ visibilidade ], "constante", IDENT,
                       [ anotacao_tipo ], "=", expressao, ";" ;
declaracao_variavel  = [ visibilidade ], "variavel", IDENT,
                       [ anotacao_tipo ], "=", expressao, ";" ;
visibilidade         = "publico" | "privado" | "protegido" ;
anotacao_tipo        = ":", tipo ;
declaracao_funcao    = [ visibilidade ], [ "assinc" ], "funcao",
                       IDENT, assinatura, bloco ;
assinatura           = "(", [ lista_parametros ], ")",
                       [ ":", tipo ] ;
lista_parametros     = parametro, { ",", parametro } ;
parametro            = IDENT, [ anotacao_tipo ], [ "=", expressao ] ;
declaracao_tipo      = "tipo", IDENT, "=", tipo, ";" ;
declaracao_tema      = "tema", IDENT, bloco ;
declaracao_componente = [ visibilidade ], "componente", IDENT,
                        [ assinatura ], bloco ;
bloco                = "{", { QUEBRA | declaracao_topo | regra_estilo
                     | declaracao_visual | comando_funcional }, "}" ;
comando_funcional    = "retorne", [ expressao ], ";"
                     | "se", "(", expressao, ")", bloco,
                       [ "senao", bloco ]
                     | "lancar", expressao, ";"
                     | "tente", bloco, { "capturar", bloco },
                       [ "finalmente", bloco ] ;
regra_estilo         = grupo_seletores, ":", bloco_estilo ;
grupo_seletores      = seletor, { ",", seletor } ;
seletor              = seletor_simples, { combinador, seletor_simples } ;
seletor_simples      = tipo_elemento, { modificador }
                     | modificador_inicial, { modificador }
                     | "*" ;
tipo_elemento        = IDENT ;
modificador_inicial  = "." IDENT | "#" IDENT | seletor_atributo
                     | seletor_estado | seletor_parte ;
modificador          = "." IDENT | "#" IDENT | seletor_atributo
                     | seletor_estado | seletor_parte ;
seletor_atributo     = "[", IDENT, [ operador_atributo, literal ], "]" ;
operador_atributo    = "=" | "!=" | "^=" | "$=" | "*=" | "~=" | "|=" ;
seletor_estado       = ":", IDENT, [ "(", [ lista_argumentos ], ")" ] ;
seletor_parte        = "::", IDENT ;
combinador           = ESPACO_SELETOR | ">" | "+" | "~" ;
bloco_estilo         = "{", { QUEBRA | declaracao_visual }, "}" ;
declaracao_visual    = declaracao_valor | regra_estilo ;
declaracao_valor     = IDENT, ":", valor_estilo, ";" ;
valor_estilo         = expressao ;
medida               = NUMERO, UNIDADE ;
referencia_token     = "$", nome_qualificado ;
lista_valores        = "[", [ valor_estilo, { ",", valor_estilo }, [ "," ] ], "]" ;
chamada_valor        = nome_qualificado, "(", [ lista_argumentos ], ")" ;
lista_argumentos     = argumento, { ",", argumento } ;
argumento            = [ IDENT, "=" ], valor_estilo ;
expressao            = termo, { operador_aritmetico, termo } ;
termo                = literal | medida | referencia_token | lista_valores
                     | chamada_valor | "(", expressao, ")" ;
operador_aritmetico  = "+" | "-" | "*" | "/" ;
literal              = STRING | STRING_MULTILINHA | NUMERO
                     | "verdadeiro" | "falso" | "nulo" ;
tipo                 = nome_qualificado | "[" tipo "]"
                     | nome_qualificado, "<", tipo, { ",", tipo }, ">" ;
nome_qualificado     = IDENT, { ".", IDENT } ;
~~~ 

### 14.1 Terminais

Os terminais de pontuação e operadores correspondem literalmente aos tokens enumerados em NSL-FASE-3-LEXICO-E-SINTAXE.md. QUEBRA, ESPACO_SELETOR, IDENT, STRING, STRING_MULTILINHA, NUMERO, UNIDADE e EOF são tokens do lexer/layout. Palavras reservadas são tokens reconhecidos por grafia, mas preservam a grafia original.

### 14.2 Limite do núcleo

O núcleo gramatical 0.2.0 cobre módulos, imports/exports, declarações básicas, regras diretas, seletores, valores, funções de valor e expressões aritméticas restritas. Recursos avançados (animações, transições, consultas, tokens temáticos avançados, estados reativos, generics avançados, efeitos e expressões Nebula) exigem extensões versionadas e não são aceitos implicitamente pelo núcleo.

### 14.3 Critério de prontidão do lexer

O inventário lexical está fechado para o núcleo acima. A análise contextual de ESPACO_SELETOR depende de estados lexicais e posição de tokens, mas nunca de heurística baseada em conteúdo semântico. O lexer entrega trivia e spans; o parser confirma a relação. A gramática e o corpus ainda não foram executados.
