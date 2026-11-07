# NSL — Base para a gramática formal

**Estado:** rascunho estrutural, não normativa  
**Versão:** 0.1.0-draft  
**Dependência:** decisões consolidadas em `NSL-FASE-2-DECISOES.md`

> Este documento não fixa ainda a grafia concreta dos delimitadores e operadores. As entrevistas aprovaram uma sintaxe original, blocos formais e expressões tipadas, mas não escolheram cada token. As produções abaixo são uma estrutura abstrata para orientar a especificação; não são uma gramática aceita para implementação.

## 1. Convenções de metassintaxe

A gramática definitiva deverá usar EBNF documentada:
- `=` define uma produção;
- `|` indica alternativa;
- `[ ... ]` indica parte opcional;
- `{ ... }` indica repetição;
- terminais serão apresentados entre aspas;
- comentários da gramática explicarão restrições que EBNF não expressa.

Os símbolos de metassintaxe não implicam que NSL utilizará os mesmos caracteres no código-fonte.

## 2. Camadas de análise

A especificação deverá separar:

1. **Camada léxica:** codificação, Unicode, identificadores, literais, comentários, operadores e delimitadores.
2. **Camada sintática:** unidades de compilação, módulos, declarações, blocos, seletores, expressões e recuperação.
3. **Camada semântica:** escopos, símbolos, tipos, unidades, propriedades, seletor válido e dependências.
4. **Camada de avaliação:** expressões constantes, expressões reativas e funções permitidas.
5. **Camada de geração:** IR, otimizações e backend.

## 3. Esqueleto abstrato de arquivo

```ebnf
unidade-compilacao = [ metadados ],
                     { declaracao-modulo },
                     { declaracao-namespace },
                     { declaracao-importacao },
                     { declaracao-exportacao },
                     { declaracao-nsl } ;

declaracao-modulo = (* sintaxe concreta pendente *) ;
declaracao-namespace = (* sintaxe concreta pendente *) ;
declaracao-importacao = (* sintaxe concreta pendente *) ;
declaracao-exportacao = (* sintaxe concreta pendente *) ;
declaracao-nsl = declaracao-seletor
               | declaracao-token
               | declaracao-variavel
               | declaracao-funcao
               | declaracao-componente
               | declaracao-tema
               | declaracao-propriedade
               | declaracao-extensao ;
```

## 4. Esqueleto abstrato de blocos e declarações

```ebnf
bloco = delimitador-abertura,
        { declaracao | comentario },
        delimitador-fechamento ;

declaracao-seletor = seletor, bloco ;
declaracao-token = identificador-tipado, valor ;
declaracao-variavel = modificadores, identificador, [ anotacao-tipo ], valor ;
declaracao-funcao = assinatura-funcao, bloco ;
declaracao-componente = assinatura-componente, bloco ;
declaracao-tema = identificador, bloco ;
declaracao-propriedade = identificador-propriedade, expressao ;
```

Os nomes `delimitador-abertura` e `delimitador-fechamento` são categorias lexicais abstratas. A escolha dos caracteres ou palavras concretas será feita em uma decisão normativa posterior.

## 5. Expressões

A gramática deverá separar níveis de precedência, em vez de aceitar uma produção ambígua única. Estrutura conceitual:

```ebnf
expressao = expressao-atribuicao ;
expressao-atribuicao = expressao-condicional ;
expressao-condicional = expressao-logica ;
expressao-logica = expressao-comparacao ;
expressao-comparacao = expressao-aditiva ;
expressao-aditiva = expressao-multiplicativa ;
expressao-multiplicativa = expressao-unaria ;
expressao-unaria = expressao-posfixa ;
expressao-posfixa = expressao-primaria ;
expressao-primaria = literal
                   | identificador
                   | chamada
                   | agrupamento
                   | expressao-composta ;
```

Esta hierarquia é apenas um índice de níveis. A especificação final deverá declarar os operadores de cada nível, associatividade, aridade, sobrecarga permitida e regras de tipos.

## 6. Seletores

O parser deverá reconhecer uma árvore de seletores composta por:
- seletor de tipo/elemento;
- seletor de identidade;
- seletor de classe ou categoria;
- seletor de atributo/propriedade;
- pseudoestado e pseudoelemento;
- combinador relacional;
- agrupamento e composição;
- condição contextual.

A AST deverá preservar a estrutura e a localização de cada componente. A análise semântica verificará se o seletor é válido no contexto NML/DOM ou no modelo de componentes aplicável.

## 7. Literais e Unicode

A especificação léxica deverá definir:
- codificação de origem e tratamento de sequência inválida;
- versão de Unicode adotada;
- normalização canônica de identificadores;
- caracteres permitidos no início e continuação;
- tratamento de caracteres visualmente confundíveis;
- escapes em identificadores e strings;
- aspas, strings multilinha e interpolação;
- separadores numéricos, expoentes, precisão e unidades;
- literais booleanos e valor ausente em português.

A implementação deverá evitar que duas grafias visualmente equivalentes gerem símbolos distintos de forma inesperada.

## 8. Regras semânticas mínimas

A especificação deverá estabelecer:
- resolução determinística de nomes e aliases;
- escopos léxicos e de módulo;
- visibilidade e exportação;
- inferência e compatibilidade de tipos;
- validação dimensional de unidades;
- validação de propriedades e valores;
- resolução de referências e dependências;
- avaliação segura de expressões constantes;
- dependências reativas e invalidação;
- diagnóstico de código inalcançável ou redundante quando aplicável.

## 9. Recuperação e diagnósticos

O parser e as ferramentas deverão produzir diagnósticos estruturados com:
- código estável;
- severidade;
- intervalo de origem;
- mensagem e contexto;
- causa provável;
- sugestão de correção, quando segura;
- informação de recuperação utilizada.

A recuperação não poderá alterar silenciosamente a semântica de um programa aceito.

## 10. Próximas decisões normativas

Antes de promover este rascunho a gramática normativa:
1. aprovar os tokens concretos de bloco e separação;
2. aprovar palavras reservadas e grafia de literais;
3. definir operadores e tabela completa de precedência;
4. definir sintaxe de seletor e combinadores;
5. definir declarações, assinaturas e anotações de tipo;
6. definir sintaxe de módulos, importação e exportação;
7. formalizar Unicode, comentários, strings e números;
8. publicar EBNF completa e corpus de exemplos válidos/inválidos;
9. implementar testes de conformidade derivados de cada produção.

## 11. Regra de status

Até que os itens acima sejam concluídos e revisados, este arquivo deve permanecer identificado como **rascunho**. Não usar este documento isoladamente como contrato para implementar um parser definitivo.


## Atualização da Fase 3

Este documento permanece como base estrutural histórica. A formalização concreta inicial de tokens, palavras reservadas, blocos, expressões e declarações foi separada em [GRAMATICA-NSL-FASE-3.md](GRAMATICA-NSL-FASE-3.md), acompanhada pelo registro de decisões em [NSL-FASE-3-LEXICO-E-SINTAXE.md](NSL-FASE-3-LEXICO-E-SINTAXE.md). Este rascunho não deve ser usado isoladamente para implementar o parser.
