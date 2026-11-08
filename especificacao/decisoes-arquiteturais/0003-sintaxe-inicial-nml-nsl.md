# Decisão arquitetural 0003 — Estrutura inicial de NML e seletores básicos de NSL

- **Estado:** aprovada como decisão inicial de sintaxe
- **Escopo:** estrutura documental NML, aninhamento, conteúdo textual e distinção inicial entre seletores NSL
- **Fase:** formalização das linguagens
- **Observação:** os exemplos deste documento registram decisões aprovadas, mas ainda não constituem uma gramática normativa completa.

## 1. NML — regiões documentais opcionais

Os blocos principais `cabecalho`, `pagina` e `rodape` são independentes e opcionais. Um documento pode conter somente um deles ou combinar dois ou todos os três. Não existe exigência de declarar todas essas regiões em cada documento.

Exemplo com somente cabeçalho:

```nml
documento NML: linguagem: NML

cabecalho: {
    ...
}
```

Exemplo com somente página:

```nml
documento NML: linguagem: NML

pagina: {
    ...
}
```

Exemplo combinando regiões:

```nml
documento NML: linguagem: NML

cabecalho: {
    ...
}

pagina: {
    ...
}

rodape: {
    ...
}
```

A possibilidade de omitir uma região não elimina as regras semânticas que venham a ser definidas para cada uma delas. A gramática deverá determinar posteriormente a validade de documentos sem nenhuma região principal.

## 2. NML — aninhamento livre de elementos

Elementos NML poderão ser aninhados livremente, inclusive dentro de outros elementos do mesmo tipo, desde que respeitem as restrições semânticas específicas de cada elemento.

```nml
pagina: {
    div: {
        div: {
            p: {
                ...
            }
        }
    }
}
```

A liberdade de aninhamento é uma regra de composição. Ela não significa que qualquer combinação será semanticamente válida: contratos de elementos, conteúdo permitido, acessibilidade e outras restrições serão especificados separadamente.

## 3. NML — conteúdo textual explícito

O conteúdo textual será declarado por meio da propriedade explícita `texto`, em vez de texto solto diretamente dentro do bloco do elemento.

```nml
t1: {
    texto: "Criado por mim"
}
```

Esse formato distingue o conteúdo textual de outras propriedades e configurações que um elemento possa receber. As regras completas de strings, escapes, interpolação e valores dinâmicos permanecem pendentes de formalização.

## 4. NSL — seletores de classe e de elemento

A NSL adotará duas formas distintas de seletor:

### 4.1 Seletor de classe

Um identificador precedido por ponto representa um seletor de classe:

```nsl
.nome: {
    elemento: configuração;
}
```

### 4.2 Seletor de elemento

Um identificador sem ponto representa um seletor de elemento:

```nsl
nome: {
    ...
}
```

Portanto, `.nome` e `nome` possuem significados diferentes: o primeiro seleciona elementos associados à classe `nome`; o segundo seleciona elementos pelo nome/tipo `nome`.

Os exemplos usam nomes e valores ilustrativos. O catálogo de propriedades, os tipos de valores, a especificidade, a cascata, os estados, os seletores compostos e a gramática completa ainda serão definidos em documentos próprios.

## 5. Limites desta decisão

Esta decisão registra somente os pontos explicitamente aprovados. Ela não define, por inferência:

- obrigatoriedade ou sintaxe final do cabeçalho `documento`;
- catálogo completo de regiões e elementos NML;
- regras finais de propriedades, atributos e expressões;
- seções adicionais e seus contratos semânticos;
- gramática normativa, precedência ou recuperação de erros;
- especificidade, cascata e herança completas da NSL;
- sintaxe final de valores, unidades, funções ou declarações NSL.

Qualquer detalhe não decidido permanece aberto para especificação posterior. A inclusão destes exemplos na documentação não declara lexer, parser, compilador ou Runtime implementados.
