# NML — Gramática EBNF da Fase 3

**Versão:** 0.1.0-draft  
**Estado:** gramática concreta proposta para revisão  
**Léxico:** [NML-FASE-3-LEXICO-E-SINTAXE.md](NML-FASE-3-LEXICO-E-SINTAXE.md)

> A gramática formaliza a sintaxe descrita no documento léxico. Regras semânticas e catálogo de elementos são definidos separadamente.

## 1. Notação

EBNF: `=` definição; `|` alternativa; `[ ... ]` opcional; `{ ... }` repetição. `QUEBRA` representa quebra de linha lógica. `IDENT`, `STRING`, `NUMERO` e demais nomes em maiúsculas são classes de token do lexer.

## 2. Unidade documental

```ebnf
arquivo              = { QUEBRA }, [ cabecalho_documento ], { QUEBRA },
                       { declaracao_raiz }, { QUEBRA }, EOF ;

cabecalho_documento  = "documento", "NML", ":", "linguagem", ":",
                       lista_linguagens, QUEBRA ;
lista_linguagens     = nome_linguagem, { ",", nome_linguagem } ;
nome_linguagem       = "NML" | "NSL" | "Nebula" | IDENT ;

declaracao_raiz      = regiao_principal | regiao_auxiliar
                     | declaracao_extensao ;
regiao_principal     = cabecalho | pagina | rodape ;
cabecalho            = "cabecalho", bloco_elementos ;
pagina               = "pagina", bloco_elementos ;
rodape               = "rodape", bloco_elementos ;
regiao_auxiliar      = metadados | sistema | estilo ;
metadados            = "metadados", bloco_dados ;
sistema              = "sistema", bloco_dados ;
estilo               = "estilo", bloco_estilo ;
declaracao_extensao  = nome_qualificado, bloco_elementos ;
```

O cabeçalho documental é opcional. A análise semântica limita cada região principal e auxiliar a uma ocorrência por arquivo.

## 3. Elementos

```ebnf
bloco_elementos      = ":", "{", QUEBRA, { item_elemento }, "}" ;
item_elemento        = propriedade | elemento | QUEBRA ;
elemento             = identificador, bloco_elementos ;
propriedade          = identificador, ":", valor, QUEBRA ;
```

A propriedade `texto` usa a produção geral de propriedade e recebe validação semântica específica.

## 4. Dados e valores

```ebnf
bloco_dados          = ":", "{", QUEBRA, { par_dado | QUEBRA }, "}" ;
par_dado             = identificador, ":", valor, QUEBRA ;
valor                = STRING | NUMERO | "verdadeiro" | "falso" | "nulo"
                     | lista | objeto_dados | referencia ;
lista                = "[", [ valor, { ",", valor }, [ "," ] ], "]" ;
objeto_dados         = "{", [ par_objeto, { ",", par_objeto }, [ "," ] ], "}" ;
par_objeto           = identificador, ":", valor ;
referencia           = "@", nome_qualificado ;
nome_qualificado     = identificador, { ".", identificador } ;
identificador        = IDENT ;
```

Referências não são executáveis; resolvem para recurso, símbolo ou módulo conforme o contexto.

## 5. NSL referenciada ou embutida

```ebnf
bloco_estilo         = ":", "{", QUEBRA, { referencia_estilo | QUEBRA }, "}" ;
referencia_estilo    = "importar", STRING, QUEBRA ;
```

Somente referências a arquivos NSL externos são aceitas no núcleo 0.1. Conteúdo NSL embutido fica reservado até que exista delimitador explícito e testável; não há token opaco sem regra léxica.

## 6. Comentários e tokens

```ebnf
comentario           = comentario_linha | comentario_bloco
                     | comentario_documentacao ;
comentario_linha     = "//", { CARACTERE }, QUEBRA ;
comentario_bloco     = "/*", { CARACTERE | comentario_bloco }, "*/" ;
comentario_documentacao = "///", { CARACTERE }, QUEBRA ;
```

O lexer reconhece strings, escapes, números, identificadores Unicode, comentários e quebras antes do parser.

## 7. Regras de layout

1. Propriedade simples termina em quebra lógica, não em ponto e vírgula.
2. Blocos abrem com dois-pontos e chave; fecham com chave.
3. Indentação interna usa quatro espaços por nível.
4. Chave de fechamento alinha-se à declaração que abriu o bloco.
5. Quebras dentro de listas/objetos podem ser ignoradas quando delimitadores estão balanceados.
6. Texto livre dentro de blocos não é permitido.
7. Bloco vazio é permitido.
8. Chaves de objetos de dados são interpretadas pelo contexto de valor, não como bloco de elemento.

## 8. Restrições semânticas

- Regiões principais e auxiliares não podem repetir-se.
- Elementos e propriedades devem existir no catálogo ou namespace importado.
- Propriedades não podem duplicar-se salvo regra de acumulação.
- `texto` deve receber string ou valor textual compatível.
- Elementos filhos precisam ser permitidos pelo contrato do pai.
- A lista de linguagens do cabeçalho não habilita automaticamente execução ou importação.
- Blocos NSL são analisados pelo parser NSL independente.

## 9. Expressões Nebula

Expressões Nebula não são incorporadas por interpolação arbitrária na gramática 0.1. A integração dinâmica usa nós de expressão tipados definidos no contrato NML–Nebula, cuja fonte é delegada ao parser Nebula oficial.

## 10. Exemplos válidos

```nml
pagina: {
    div: {
        t1: {
            texto: "Criado por mim"
        }
    }
}
```

```nml
cabecalho: {
    titulo: {
        texto: "Portal"
    }
}

rodape: {
    div: {
        t1: {
            texto: "2026"
        }
    }
}
```

## 11. Exemplos inválidos

Texto livre sem `texto`; propriedade sem valor; bloco sem fechamento; chave extra; região principal repetida; propriedade duplicada não acumulável; identificador malformado; string não terminada; filho semanticamente proibido.

## 12. Estado

Esta EBNF é uma proposta formal para o núcleo estático 0.1. Recursos dinâmicos avançados e NSL embutida dependem de contratos adicionais. A gramática não foi executada por parser e não comprova implementação.


## 13. Adendo normativo de fechamento

Este adendo resolve os pontos contextuais necessários ao lexer/parser:

- `texto` permanece terminal IDENT. A gramática não cria terminal reservado para essa grafia.
- `documento NML: linguagem: ...` é reconhecido somente no início do arquivo. `NSL` e `Nebula` são valores da lista de linguagens, não palavras reservadas globais.
- Cabeçalho documental e regiões são declarações raiz; a mesma grafia em posição de propriedade/elemento é resolvida pelo contexto gramatical, sem reclassificação lexical.
- Uma quebra física dentro de colchetes, parênteses ou objeto de valor não encerra a produção; fora desses delimitadores, propriedade termina em quebra lógica.
- String vazia é válida. Lista e objeto vazios são válidos. Chaves de objeto duplicadas são erro semântico.
- Objeto de dados somente ocorre na posição de valor. Bloco de elemento somente ocorre após o identificador de elemento e seu dois-pontos.
- Região raiz duplicada, propriedade duplicada, conteúdo misto incompatível e tipo de propriedade inválido são erros semânticos, não erros do lexer.
- Todo token terminal deve estar definido no inventário lexical 0.1.0. O lexer não resolve catálogo de elementos/propriedades.

A gramática é considerada documentalmente fechada para iniciar o lexer, sujeita a correções descobertas por testes. Parser e corpus ainda não foram implementados/executados.
