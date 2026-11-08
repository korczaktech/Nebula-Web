# NML — Fase 3: formalização léxica e sintática

**Versão:** 0.1.0-draft  
**Estado:** proposta normativa consolidada para revisão  
**Implementação pretendida:** 100% em Nebula  
**Decisões aprovadas:** [0003 — Sintaxe inicial de NML e NSL](decisoes-arquiteturais/0003-sintaxe-inicial-nml-nsl.md)

> Este documento fecha uma proposta técnica para a primeira versão da linguagem. As decisões expressamente aprovadas pelo autor são definitivas. Regras complementares necessárias à consistência do compilador são propostas normativas editoriais. A publicação não significa que exista implementação.

## 1. Identidade e responsabilidades

NML descreve estrutura, conteúdo e composição de documentos e interfaces. Não é HTML traduzido. NSL controla apresentação visual e permanece uma linguagem separada.

A primeira versão do frontend NML reconhece cabeçalho documental opcional, regiões independentes, elementos nomeados e aninháveis, propriedades declarativas, conteúdo textual explícito, metadados, referências NSL e valores tipados.

Comportamentos dinâmicos, eventos, navegação, componentes parametrizados e expressões Nebula são previstos pela arquitetura, mas só podem ser aceitos nos contextos e perfis definidos nesta especificação e nos contratos de integração. Texto arbitrário nunca é interpretado como código.

## 2. Decisões aprovadas

1. `cabecalho`, `pagina` e `rodape` são regiões opcionais e independentes. Um documento pode conter somente uma delas ou combiná-las.
2. Elementos podem ser aninhados livremente, inclusive elementos do mesmo tipo, sujeitos à validação semântica.
3. Texto de um elemento é declarado explicitamente pela propriedade `texto`.
4. Elementos usam nome, dois-pontos e bloco entre chaves, como `div: { ... }`.
5. A apresentação é responsabilidade da NSL, não da gramática NML.

## 3. Codificação e identificadores

- Codificação UTF-8 estrita; sequências inválidas são erros léxicos.
- Versão Unicode de referência desta proposta: Unicode 15.1.0. Atualizações exigem revisão versionada.
- Identificadores seguem `XID_Start` e `XID_Continue`, com sublinhado permitido no início e continuação.
- Identificadores são sensíveis a maiúsculas/minúsculas e comparados em NFC. A grafia original é preservada.
- `camelCase` é convenção recomendada para nomes compostos.
- Caracteres de controle e invisíveis proibidos são rejeitados.
- Identificadores visualmente confundíveis geram diagnóstico de segurança; não são automaticamente fundidos.
- Palavras reservadas não podem ser usadas como identificadores simples. Extensões usam namespaces.

## 4. Espaços, linhas e indentação

- Espaços e quebras de linha separam tokens; tabulações não são permitidas para indentação.
- Indentação obrigatória de quatro espaços por nível.
- Elementos irmãos mantêm o mesmo nível.
- Chave de fechamento alinha-se à coluna da declaração que abriu o bloco.
- Chaves delimitam sintaxe; indentação divergente continua sendo erro.
- Quebra de linha separa propriedades quando o parser está em posição de término. Valores compostos podem continuar em linhas seguintes dentro de delimitadores balanceados.
- Comentários e linhas vazias não alteram indentação.
- NML 0.1 não exige ponto e vírgula após propriedades. Ponto e vírgula fica reservado para contextos de expressão futuros.

## 5. Comentários

- `//` inicia comentário até o fim da linha.
- `/* ... */` delimita comentário de bloco; blocos aninhados são permitidos até limite documentado.
- `///` inicia comentário de documentação associado à próxima declaração.
- Comentários não alteram semântica.
- Diretivas de compilação não são comentários comuns e exigem prefixo reservado em versão futura.

## 6. Strings e literais

- Strings de uma linha aceitam aspas duplas ou simples.
- Strings multilinha usam aspas triplas correspondentes.
- Escapes mínimos: barra invertida, aspas delimitadoras, nova linha, retorno, tabulação e escape Unicode escalar.
- Escape inválido, sequência Unicode inválida e string não terminada são erros.
- Literais numéricos: inteiro decimal, hexadecimal, binário, octal e decimal fracionário/científico.
- Separadores numéricos, quando usados, só aparecem entre dígitos.
- Valores lógicos: `verdadeiro` e `falso`; ausência explícita: `nulo`.
- Unidades e cores são tipadas; sufixos arbitrários não são inferidos como unidades.
- Interpolação fica reservada até que a gramática de expressões Nebula embutidas seja versionada; `${...}` não é aceito implicitamente nesta versão.

## 7. Estrutura documental

### 7.1 Cabeçalho

A forma mostrada no exemplo inicial — `documento NML: linguagem: NML, NSL, Nebula` — permanece um exemplo de identificação. A gramática desta versão permite cabeçalho omitido; quando presente, ocorre no início do arquivo e no máximo uma vez.

### 7.2 Regiões principais

`cabecalho`, `pagina` e `rodape`:
- são opcionais e independentes;
- podem aparecer em qualquer ordem no nível superior;
- podem aparecer no máximo uma vez cada;
- podem conter elementos e propriedades permitidos pelo catálogo semântico;
- não exigem a presença das demais regiões.

Documento sem regiões principais é sintaticamente válido como unidade vazia, mas pode receber aviso de ausência de conteúdo renderizável.

### 7.3 Regiões auxiliares

`metadados`, `sistema` e `estilo` são regiões auxiliares opcionais, no máximo uma vez por documento. `estilo` contém referências ou conteúdo NSL somente conforme o contrato de integração. Conteúdo NSL não é interpretado como NML.

## 8. Elementos e propriedades

Forma geral:

```nml
nomeDoElemento: {
    propriedade: valor
    elementoFilho: {
        ...
    }
}
```

- O nome identifica elemento nativo ou componente importado.
- O bloco contém propriedades, conteúdo e filhos.
- Elementos podem ser aninhados sem limite artificial; o compilador aplica limites de recursos.
- Validade de cada filho depende do contrato semântico do pai.
- Propriedades não podem se repetir, salvo quando declaradas acumuláveis.
- Propriedades desconhecidas são erro; extensões precisam ser importadas/qualificadas.
- Propriedades escalares usam `nome: valor`; valores compostos usam listas ou blocos definidos pela gramática.

### 8.1 Conteúdo textual

O conteúdo textual é sempre explícito:

```nml
t1: {
    texto: "Criado por mim"
}
```

Texto solto dentro do bloco não é permitido. `texto` é um identificador comum no léxico e uma propriedade contextual especial somente na semântica de elementos.

### 8.2 Listas e objetos

Listas usam colchetes e vírgulas. Objetos usam pares nome-valor entre chaves somente em posições de valor autorizadas. Blocos de elemento e objetos de dados são nós AST diferentes, mesmo que ambos usem chaves.

## 9. Valores e referências

A primeira versão admite strings, inteiros, decimais, booleanos, `nulo`, listas, objetos de dados e referências tipadas a recursos/módulos.

Referências externas são resolvidas e validadas pelo compilador/empacotador. Caminhos de recursos não são executados como código. Valores dinâmicos Nebula são aceitos apenas por nós de expressão tipados definidos no contrato NML–Nebula, não por interpolação textual livre.

## 10. Segurança e limites

- NML não executa scripts arbitrários nem concede acesso direto a hardware, sistema operacional ou syscalls.
- Recursos externos passam por políticas de origem, integridade, permissões e mecanismos do navegador.
- Integração especial ZenithOS/Portunus não altera a semântica geral de NML; somente adaptadores autorizados podem expor capacidades mediadas pelo kernel.
- Profundidade, tamanho de string, número de nós, tamanho de arquivo e expansão de referências possuem limites documentados.
- Erros de limite geram diagnósticos determinísticos, nunca crashes.

## 11. Compatibilidade e evolução

A gramática é versionada. Versões minor podem adicionar construções compatíveis; mudanças incompatíveis exigem versão major e ferramenta de migração. Palavras reservadas novas não podem capturar silenciosamente identificadores válidos de versões estáveis.

## 12. Documentos normativos relacionados

- [Gramática EBNF NML](GRAMATICA-NML-FASE-3.md).
- [AST, tipos e semântica NML](NML-MODELO-SEMANTICO-E-AST.md).
- [Contratos NML–NSL–Nebula](CONTRATOS-NML-NSL-NEBULA.md).
- [Corpus NML](testes/NML-FASE-3-CORPUS-CONFORMIDADE.md).

## 13. Critério de fechamento

A Fase 3 documental NML estará especificada quando léxico, EBNF, semântica, diagnósticos e corpus forem revisados como conjunto. Aprovação documental não equivale à implementação do lexer/parser nem à aprovação de testes executados.


---

## 14. Inventário léxico do núcleo 0.1.0

### 14.1 Palavras reservadas do documento

| Palavra | Contexto |
|---|---|
| `documento` | cabeçalho documental |
| `NML` | identificador de linguagem no cabeçalho |
| `linguagem` | campo do cabeçalho |
| `cabecalho` | região principal |
| `pagina` | região principal |
| `rodape` | região principal |
| `metadados` | região auxiliar |
| `sistema` | região auxiliar |
| `estilo` | região auxiliar |
| `importar` | referência externa de estilo |
| `verdadeiro`, `falso` | literais lógicos |
| `nulo` | literal de ausência |

Palavras reservadas são case-sensitive. `texto` não é reservada. `NSL` e `Nebula` são nomes de linguagem reconhecidos no cabeçalho; fora dele são identificadores. Identificadores de elemento/propriedade são emitidos como IDENT.

### 14.2 Pontuação e delimitadores

| Token | Uso |
|---|---|
| `:` | introduz bloco ou propriedade |
| `{` e `}` | blocos de elementos/dados |
| `[` e `]` | listas |
| `(` e `)` | agrupamentos reservados a extensões |
| `,` | separa itens de listas/cabeçalho |
| `.` | qualificação de nomes |
| `@` | inicia referência tipada |
| `"` e `'` | delimitadores de string |

### 14.3 Classes de token

O lexer deve produzir pelo menos: IDENT, STRING, STRING_MULTILINHA, NUMERO_INTEIRO, NUMERO_DECIMAL, BOOLEANO, NULO, DOIS_PONTOS, ABRE_CHAVE, FECHA_CHAVE, ABRE_COLCHETE, FECHA_COLCHETE, VIRGULA, PONTO, ARROBA, QUEBRA, INDENTACAO, DEDENTACAO, COMENTARIO e EOF.

Tokens de comentário podem ser descartados após a associação de comentários de documentação; tokens de quebra e indentação preservam a origem para diagnósticos e layout.

### 14.4 Precedência de reconhecimento

1. Comentários e strings são reconhecidos antes de pontuação interna.
2. Números são reconhecidos antes de identificadores quando começam por dígito.
3. Palavras reservadas são reconhecidas após formar o identificador completo.
4. Operadores/delimitadores são reconhecidos pelo maior prefixo válido.
5. Caractere não classificado gera erro léxico com offset e caractere de origem.

### 14.5 Política de layout

O lexer preserva quebra física e calcula quebra lógica. Quebra dentro de string multilinha não produz separador. Quebra dentro de lista/objeto pode ser ignorada pelo parser enquanto o delimitador estiver aberto. Indentação de fechamento deve corresponder exatamente ao nível de abertura.


## 15. Fechamento normativo para implementação do lexer

1. `texto` é IDENT, nunca TOKEN_TEXTO nem palavra reservada global.
2. `documento`, `linguagem` e `NML` são terminais contextuais do cabeçalho; `NSL` e `Nebula` são valores de nome de linguagem nesse contexto.
3. `cabecalho`, `pagina`, `rodape`, `metadados`, `sistema` e `estilo` são reconhecidos como regiões somente na posição de declaração raiz.
4. O lexer emite IDENT para nomes de elementos, propriedades, componentes e chaves de objetos.
5. O lexer distingue quebra física de quebra lógica e preserva indentação original; a validação estrutural é executada pelo estágio de layout/parser.
6. String vazia é token STRING válido. Interpolação não pertence ao núcleo 0.1.0.
7. Os tokens numéricos não absorvem sufixos arbitrários. Unidades visuais pertencem exclusivamente ao léxico NSL.
8. Os documentos léxico, EBNF, semântica e corpus devem ser lidos em conjunto. O corpus não está executado.

**Pronto para implementação:** sim, como contrato documental do lexer NML. Isso não declara lexer implementado ou testado.
