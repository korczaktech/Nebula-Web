# NSL — Fase 3: formalização léxica e sintática

**Estado:** proposta técnica consolidada para revisão e testes  
**Versão:** 0.1.0-draft  
**Base:** decisões aprovadas nas Fases 1 e 2 e nos lotes 1–3 da Fase 3  
**Idioma das palavras reservadas:** português brasileiro  
**Nomenclatura de identificadores:** `camelCase` como padrão  
**Implementação pretendida:** Nebula

> Este documento converte decisões de produto em uma proposta concreta de sintaxe. As escolhas explicitamente respondidas pelo usuário são identificadas como aprovadas. Tokens concretos escolhidos para preencher lacunas técnicas são identificados como propostas editoriais, ainda sujeitos a testes de ambiguidade e conformidade. Nada aqui afirma que o compilador já implemente NSL.

## 1. Decisões aprovadas

### 1.1 Lote 1 — identidade e estrutura

- NSL possui identidade sintática e semântica própria.
- A interoperabilidade com CSS deverá ser completa sempre que tecnicamente possível, por conversão explícita e diagnóstico de perdas.
- Evolução, compatibilidade, palavras reservadas, namespaces e diagnósticos terão governança formal.
- Português é o idioma padrão das palavras reservadas.
- Identificadores seguem `camelCase` como padrão.
- Unicode, normalização, segurança contra caracteres confundíveis e interoperabilidade terão suporte formal.
- Blocos usam delimitadores explícitos e indentação validada; aninhamento e escopos são definidos pela gramática.
- Argumentos, coleções e expressões possuem delimitadores próprios e regras completas.
- Strings, números, valores lógicos, ausência de valor, comentários e diretivas possuem sistemas completos.
- Erros terão localização precisa, contexto, recuperação, sugestões e diagnósticos múltiplos.
- Ferramentas externas terão integração completa por protocolos e formatos documentados.

### 1.2 Lote 2 — forma concreta

- A forma de uma declaração visual é funcional: `propriedade(valor);`.
- As demais estruturas terão sintaxe própria, integrada e não serão cópias diretas de CSS.
- Seletores combinarão elementos, classes, identificadores, atributos, estados e relações.
- Propriedades e funções terão nomes em português como padrão, com aliases configuráveis para interoperabilidade.
- Valores, medidas, cores, temas, animações, efeitos, módulos e componentes terão modelos tipados e extensíveis.
- Operadores, funções nativas, aliases e abreviações terão regras formais.

### 1.3 Lote 3 — semântica e execução

O usuário confirmou opção 4 para todas as decisões do lote. Isso reafirma os requisitos completos de compilação e execução incremental, reatividade, tipos, validação, componentes, composição, segurança, interoperabilidade, backends, integração controlada com ZenithOS/Portunus, extensibilidade, governança e maturidade de produção. Essas confirmações não são evidência de implementação.

## 2. Convenções desta proposta

As grafias concretas abaixo são decisões editoriais técnicas para tornar a gramática testável. Foram escolhidas para:
- priorizar português sem acentos em palavras reservadas, facilitando digitação e portabilidade;
- preservar os nomes originais de elementos e APIs externas quando exigido pela plataforma;
- manter chamadas de propriedade no formato funcional aprovado;
- usar chaves para delimitar blocos e exigir indentação consistente;
- permitir análise léxica determinística sem depender de heurísticas frágeis.

## 3. Codificação e identificadores

1. Arquivos NSL são codificados em UTF-8 sem sequências inválidas.
2. Identificadores são sensíveis a maiúsculas e minúsculas.
3. A forma recomendada é `camelCase`; nomes de tipos e componentes podem usar `PascalCase` por convenção de categoria.
4. Identificadores Unicode seguem as propriedades `XID_Start` e `XID_Continue` da versão Unicode fixada por cada versão da especificação.
5. A comparação semântica usa normalização NFC.
6. O compilador deve diagnosticar identificadores visualmente confundíveis (confusables) e caracteres invisíveis ou de controle suspeitos.
7. Palavras reservadas não podem ser usadas como identificadores simples. Um mecanismo de escape de identificador poderá ser definido no perfil de interoperabilidade; não poderá mudar o significado de palavras reservadas em código existente.
8. O compilador deve preservar a grafia original para diagnósticos e source maps.

## 4. Tokens e pontuação

### 4.1 Delimitadores

| Token | Papel |
|---|---|
| `(` `)` | argumentos, agrupamento e chamadas |
| `{` `}` | abertura e fechamento de blocos |
| `[` `]` | listas, tuplas e indexação |
| `.` | acesso qualificado |
| `,` | separação de itens |
| `;` | encerramento explícito de declaração simples |
| `:` | anotação de tipo e pares de metadados, nunca substitui a chamada funcional de propriedade |
| `@` | metadados e anotações |
| `?` | operador condicional em contextos definidos |
| `...` | expansão variádica |
| `..` | intervalo exclusivo, quando usado em contexto de intervalo |

### 4.2 Operadores

| Categoria | Tokens propostos |
|---|---|
| Aritméticos | `+` `-` `*` `/` `%` `**` |
| Atribuição | `=` `+=` `-=` `*=` `/=` |
| Comparação | `==` `!=` `<` `<=` `>` `>=` |
| Lógicos | `e` `ou` `nao` |
| Nulidade | `??` `?.` |
| Composição | `|>` |
| Função/lambda | `=>` |
| Referência | `&` |
| Desreferência, se permitida pelo contexto tipado | `*` |

A sobrecarga de operadores é limitada por contratos de tipos e não pode alterar precedência, aridade ou associatividade. O operador `*` é resolvido pelo contexto sintático e semântico, sem ambiguidade lexical.

### 4.3 Precedência inicial

Da maior para a menor precedência:

1. acesso, chamada, indexação e acesso opcional;
2. potência;
3. unários (`+`, `-`, `nao`, referência);
4. multiplicação, divisão e resto;
5. adição e subtração;
6. intervalos;
7. comparações;
8. igualdade;
9. `e`;
10. `ou`;
11. coalescência de nulo;
12. condicional;
13. composição/pipeline;
14. atribuição.

A tabela final deverá declarar associatividade por operador. Comparações encadeadas não serão aceitas implicitamente; devem ser agrupadas ou combinadas com operadores lógicos.

## 5. Palavras reservadas

### 5.1 Núcleo reservado

A primeira proposta de palavras reservadas é:

```text
modulo importar exportar como publico privado protegido
constante variavel funcao retorne tipo generico onde
se senao escolha caso para enquanto interromper continuar
tente capturar finalmente lancar assinc aguardar
verdadeiro falso nulo ausente
estilo seletor componente propriedade tema token estado
evento variante slot escopo global local interno externo
quando com de em entre incluir excluir
e ou nao
```

### 5.2 Palavras contextuais

Os termos abaixo são reservados apenas em posições gramaticais específicas, podendo ser identificadores em outros contextos se não houver conflito:

```text
herdar compor estender animacao transicao consulta recurso
fallback prioridade camada origem densidade responsivo
```

### 5.3 Política de expansão

- A lista de palavras reservadas é versionada.
- Novas palavras não podem invalidar silenciosamente identificadores válidos de versões estáveis.
- Palavras futuras podem ser reservadas em namespaces de extensão, não no espaço global, até aprovação de uma versão principal.
- O parser distingue palavras contextuais por posição gramatical.
- A ferramenta de migração deve sugerir renomeação quando uma nova palavra reservada entrar em conflito com código antigo.

## 6. Literais e comentários

### 6.1 Booleanos e ausência

- `verdadeiro` e `falso` são literais booleanos.
- `nulo` representa ausência intencional de valor.
- `ausente` representa valor não fornecido em estruturas que distinguem ausência de nulidade.
- Valor não inicializado é um estado de análise/execução, não um literal de origem.

### 6.2 Strings

- Aspas duplas e simples delimitam strings de uma linha.
- Aspas triplas delimitam strings multilinha.
- Escapes incluem aspas, barra invertida, nova linha, tabulação e escape Unicode.
- Interpolação usa uma forma delimitada e balanceada, proposta como `"${expressao}"`.
- Strings cruas e templates são categorias distintas na AST.

### 6.3 Números e unidades

- Inteiros decimais, binários, octais e hexadecimais são reconhecidos.
- Separadores numéricos são permitidos entre dígitos, nunca no início/fim nem duplicados.
- Decimais e notação científica são suportados.
- A unidade visual deve ser tipada; a gramática não trata qualquer sufixo arbitrário como unidade válida.
- Overflow, precisão e conversões são definidos por tipo e perfil de compilação.

### 6.4 Comentários

- `//` inicia comentário de linha.
- `/* ... */` delimita comentário de bloco aninhável.
- `///` inicia documentação de declaração.
- Comentários não alteram a semântica, exceto diretivas reconhecidas explicitamente pelo compilador.

## 7. Blocos, whitespace e indentação

1. Blocos são delimitados por `{` e `}`.
2. A indentação é obrigatória e validada; cada nível de bloco usa a mesma largura de indentação dentro do arquivo.
3. Tabulações e espaços não podem ser misturados no mesmo nível de indentação.
4. Uma chave de fechamento deve alinhar-se à coluna inicial da declaração que abriu o bloco.
5. Chaves não substituem a validação de indentação.
6. Declarações simples terminam com `;`.
7. Declarações de bloco não recebem ponto e vírgula após a chave de fechamento.
8. Quebras de linha podem separar declarações apenas quando a gramática permite; não encerram chamadas incompletas.
9. O parser deve rejeitar chaves não balanceadas, indentação inconsistente e declarações sem terminador obrigatório.
10. Não existe palavra de fechamento equivalente a `fim`; o fechamento estrutural é feito por `}`.

## 8. Forma funcional de propriedades

A forma aprovada para uma declaração de propriedade é:

```nsl
cor(cor.hex("#3366cc"));
largura(medida.px(240));
altura(medida.auto());
margem(medida.rem(1));
opacidade(0.85);
```

Regras:
- o nome da propriedade é uma função do namespace visual corrente;
- a chamada recebe um valor ou expressão tipada;
- argumentos nomeados são permitidos quando a propriedade define mais de um parâmetro;
- cada chamada de propriedade é uma declaração e termina com `;`;
- propriedades desconhecidas são erro, aviso ou extensão apenas conforme o namespace e a política de módulos;
- aliases em inglês ou nomes CSS são resolvidos por tabela de interoperabilidade, nunca por heurística.

## 9. Seletores

Seletores são expressões tipadas construídas por funções do namespace `seletor`. Esta decisão evita reutilizar a pontuação e os combinadores textuais do CSS.

Exemplos propostos:

```nsl
seletor.elemento("button")
seletor.classe("botaoPrincipal")
seletor.id("acaoPrimaria")
seletor.atributo("aria-expanded", igualA("true"))
seletor.estado("foco")
seletor.descendente(
    seletor.elemento("painel"),
    seletor.classe("titulo")
)
seletor.filho(
    seletor.componente("Cartao"),
    seletor.slot("conteudo")
)
seletor.grupo(
    seletor.classe("botaoPrimario"),
    seletor.classe("botaoSecundario")
)
```

A AST do seletor preserva tipo, relação, escopo, origem e posição. A validação semântica verifica se o seletor pode existir no contexto NML, DOM ou Web Components.

## 10. Estrutura de arquivo e declarações

A forma de alto nível proposta:

```nsl
modulo interface.principal {
    importar tema.claro como temaClaro;

    constante corDestaque = cor.hex("#3366cc");

    estilo(
        seletor.classe("botaoPrincipal")
    ) {
        cor(cor.hex("#ffffff"));
        fundo(corDestaque);
        largura(medida.px(240));
    }
}
```

Regras:
- cada arquivo pertence a um módulo explícito ou a um módulo implícito derivado do projeto;
- importações aparecem antes de declarações executáveis do módulo;
- aliases são explícitos;
- exportação e visibilidade são verificadas semanticamente;
- declarações de estilo aceitam um seletor tipado e um bloco de propriedades;
- componentes, temas, tokens, funções e propriedades personalizadas usam declarações próprias, formalizadas em gramática derivada.

## 11. Gramática EBNF inicial

A EBNF abaixo formaliza o núcleo já proposto. Produções marcadas como extensão serão detalhadas em documentos derivados.

```ebnf
arquivo              = { quebra }, [ modulo ], { declaracao }, { quebra } ;
modulo               = "modulo", nome_qualificado, bloco ;
bloco                = "{", { quebra | declaracao }, "}" ;
declaracao           = importacao
                     | exportacao
                     | declaracao_constante
                     | declaracao_variavel
                     | declaracao_estilo
                     | declaracao_funcao
                     | declaracao_tipo
                     | declaracao_tema
                     | declaracao_componente ;
importacao           = "importar", nome_qualificado, [ "como", identificador ], ";" ;
exportacao           = "exportar", identificador, ";" ;
declaracao_constante = "constante", identificador, [ anotacao_tipo ],
                       "=", expressao, ";" ;
declaracao_variavel  = "variavel", identificador, [ anotacao_tipo ],
                       "=", expressao, ";" ;
anotacao_tipo        = ":", tipo ;
declaracao_estilo    = "estilo", "(", seletor_expr, ")", bloco ;
bloco_estilo         = "{", { quebra | declaracao_propriedade }, "}" ;
declaracao_propriedade
                     = identificador_propriedade, "(", lista_argumentos_opcional, ")", ";" ;
declaracao_funcao    = "funcao", identificador, assinatura, bloco ;
declaracao_tipo      = "tipo", identificador, "=", tipo, ";" ;
declaracao_tema      = "tema", identificador, bloco ;
declaracao_componente
                     = "componente", identificador, assinatura_opcional, bloco ;
assinatura           = "(", [ lista_parametros ], ")", [ ":", tipo ] ;
lista_parametros     = parametro, { ",", parametro } ;
parametro            = identificador, [ ":", tipo ], [ "=", expressao ] ;
lista_argumentos_opcional
                     = [ argumento, { ",", argumento } ] ;
argumento            = [ identificador, "=" ], expressao ;
seletor_expr         = chamada_seletor ;
chamada_seletor      = nome_qualificado, "(", [ lista_argumentos_opcional ], ")" ;
expressao            = atribuicao ;
atribuicao           = condicional, [ operador_atribuicao, atribuicao ] ;
condicional          = logica_ou, [ "?", expressao, ":", expressao ] ;
logica_ou            = logica_e, { "ou", logica_e } ;
logica_e             = igualdade, { "e", igualdade } ;
igualdade            = comparacao, { ( "==" | "!=" ), comparacao } ;
comparacao           = intervalo, { ( "<" | "<=" | ">" | ">=" ), intervalo } ;
intervalo            = aditiva, [ "..", aditiva ] ;
aditiva              = multiplicativa, { ( "+" | "-" ), multiplicativa } ;
multiplicativa       = unaria, { ( "*" | "/" | "%" ), unaria } ;
unaria               = [ "+" | "-" | "nao" ], posfixa ;
posfixa              = primaria, { acesso | chamada | indexacao } ;
acesso               = ".", identificador ;
chamada              = "(", [ lista_argumentos_opcional ], ")" ;
indexacao            = "[", expressao, "]" ;
primaria             = literal | identificador | agrupamento | lista ;
agrupamento          = "(", expressao, ")" ;
lista                = "[", [ expressao, { ",", expressao } ], "]" ;
literal              = numero | string | "verdadeiro" | "falso" | "nulo" | "ausente" ;
nome_qualificado     = identificador, { ".", identificador } ;
```

## 12. Regras de validação semântica

- Uma chamada de propriedade só pode ocorrer dentro de um bloco de estilo ou em outro contexto explicitamente autorizado.
- O nome da propriedade é resolvido no catálogo e nos namespaces importados.
- O valor deve satisfazer o tipo, domínio, unidade e restrições da propriedade.
- A chamada funcional de propriedade não pode ser confundida com chamada de função de usuário; a resolução usa contexto gramatical e símbolo tipado.
- Seletor e bloco de estilo formam uma declaração indivisível na AST.
- Importações são resolvidas de forma determinística e respeitam ciclos conforme política de módulos.
- Nenhuma conversão CSS é considerada semântica se perder informação sem diagnóstico.
- Recursos específicos de ZenithOS só são acessíveis pelo contexto autorizado Portunus/ZenithOS, por contratos e syscalls mediados pelo kernel.

## 13. Casos válidos iniciais

```nsl
modulo exemplo.basico {
    constante espacamentoBase = medida.rem(1);

    estilo(seletor.elemento("button")) {
        cor(cor.hex("#ffffff"));
        fundo(cor.hex("#2457a6"));
        margem(espacamentoBase);
    }
}
```

```nsl
modulo exemplo.componentes {
    estilo(
        seletor.descendente(
            seletor.componente("Cartao"),
            seletor.classe("titulo")
        )
    ) {
        tamanhoFonte(medida.rem(1.25));
        pesoFonte(700);
    }
}
```

## 14. Casos inválidos iniciais

```nsl
// Inválido: declaração de propriedade fora de bloco autorizado.
cor(cor.hex("#fff"));
```

```nsl
// Inválido: chamada de propriedade sem terminador.
estilo(seletor.elemento("button")) {
    cor(cor.hex("#fff"))
}
```

```nsl
// Inválido: indentação interna inconsistente.
estilo(seletor.elemento("button")) {
  cor(cor.hex("#fff"));
    fundo(cor.hex("#000"));
}
```

```nsl
// Inválido: delimitador de bloco não balanceado.
modulo exemplo {
    estilo(seletor.elemento("button")) {
        cor(cor.hex("#fff"));
}
```

## 15. Pendências técnicas que não exigem decisão de produto

As próximas especificações técnicas devem fechar, sem nova entrevista geral:
- tabela completa de tokens e estados do lexer;
- gramática integral de declarações, funções, tipos, componentes, temas e animações;
- associatividade final e regras de sobrecarga;
- gramática de interpolação e strings cruas;
- catálogo normativo de propriedades, tipos, unidades e funções;
- resolução formal de namespaces e módulos;
- algoritmo de especificidade, cascata, escopo e herança;
- formato de AST, IR, source maps e diagnósticos;
- corpus de conformidade positivo, negativo, fuzzing e testes diferenciais;
- critérios de compatibilidade CSS e matriz de backends.

Esses detalhes serão decididos tecnicamente e documentados. Uma nova pergunta ao usuário só será feita se surgir uma escolha de produto genuína que não possa ser resolvida por consistência técnica.

## 16. Critério de promoção a gramática normativa

Este documento continua em rascunho até que:
1. a gramática completa seja validada sem conflitos;
2. cada produção tenha casos positivos e negativos;
3. o lexer e parser de referência aceitem e rejeitem o corpus esperado;
4. os diagnósticos sejam estáveis e testados;
5. a interoperabilidade e as regras de versão estejam documentadas;
6. a revisão formal aprove a versão normativa.

A existência deste documento não significa que lexer, parser, compilador ou runtime estejam implementados.


## 17. Documentos relacionados

- [Gramática EBNF da Fase 3](GRAMATICA-NSL-FASE-3.md).
- [Corpus de conformidade da Fase 3](testes/NSL-FASE-3-CORPUS-CONFORMIDADE.md).

A gramática foi verificada estaticamente quanto a referências não definidas e produções duplicadas. Essa verificação documental não substitui a execução do corpus por lexer/parser de referência.


---

## 18. Revisão sintática 0.2.0 — seletores diretos

A sintaxe básica aprovada pelo autor substitui a apresentação de seletores por chamadas funcionais nos exemplos anteriores:

- seletor de classe: `.nome: { ... }`;
- seletor de elemento: `nome: { ... }`;
- declaração visual: `propriedade: valor;`.

Exemplo:

```nsl
.nome: {
    cor: "#ffffff";
    tamanhoFonte: 16px;
}

botao: {
    fundo: "#202020";
}
```

A forma `estilo(seletor.elemento(...)) { ... }` documentada em exemplos anteriores é histórica e deixa de ser a sintaxe canônica de autoria. A gramática revisada na seção 12 de `GRAMATICA-NSL-FASE-3.md` é a referência para regras de seletores e declarações visuais.

As extensões `#id`, seletores de atributo, relações, estados e partes são propostas técnicas complementares, não substituem as duas formas básicas aprovadas. A revisão continua draft até revisão formal e execução do corpus.
