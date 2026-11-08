# Diagnósticos e recuperação — NML e NSL

**Versão:** 0.1.0-draft  
**Estado:** contrato compartilhado proposto

## 1. Objetivo

Definir formato comum de diagnósticos sem compartilhar gramáticas ou semântica entre NML e NSL. Cada frontend mantém seus próprios códigos e regras; ambos usam o mesmo envelope público de diagnóstico.

## 2. Estrutura obrigatória

Todo diagnóstico contém:
- código estável e versionado;
- severidade: erro, aviso ou informação;
- linguagem: NML, NSL ou integração;
- fase: codificação, léxico, layout, sintaxe, resolução, semântica, tipos, integração ou backend;
- mensagem curta e explicação;
- arquivo e intervalo de origem;
- notas relacionadas com outros intervalos;
- sugestão de correção opcional;
- identificador de diagnóstico relacionado, quando fizer parte de uma cadeia.

Offsets internos são bytes UTF-8; linha/coluna públicas são baseadas em caracteres Unicode. Intervalos são semiabertos: início inclusivo e fim exclusivo.

## 3. Espaços de códigos

| Prefixo | Área |
|---|---|
| NML-L | léxico NML |
| NML-S | sintaxe/layout NML |
| NML-R | resolução/estrutura NML |
| NML-T | tipos/propriedades NML |
| NML-A | acessibilidade/segurança NML |
| NSL-L | léxico NSL |
| NSL-S | sintaxe/layout NSL |
| NSL-R | resolução/seletor NSL |
| NSL-T | tipos/unidades/propriedades NSL |
| NSL-C | cascata/escopo NSL |
| INT | integração NML–NSL–Nebula |

O identificador numérico de um código não é reutilizado para significado diferente. Mudanças de texto não alteram o código se a condição detectada permanecer compatível.

## 4. Severidade e compilação

- Erro impede que o artefato afetado seja marcado como válido.
- Aviso não impede geração quando a política do projeto permite continuar.
- Informação descreve comportamento ou fallback aplicado.
- Um arquivo com erro pode produzir AST parcial para editor, mas não AST semanticamente válida.
- Modo estrito pode elevar avisos configuráveis a erros; não pode rebaixar erros de segurança ou integridade.

## 5. Recuperação

O lexer deve recuperar-se em limites seguros após caracteres inválidos sem perder a localização original. Parser usa pontos de sincronização próprios de cada gramática: fechamento de bloco, quebra lógica e início de declaração. Recuperação não pode consumir silenciosamente o próximo bloco válido.

Nós sintéticos são marcados como inválidos e mantêm a origem do erro. Ferramentas não podem confundir árvore parcial com compilação bem-sucedida.

## 6. Diagnósticos mínimos obrigatórios

| Código | Condição |
|---|---|
| NML-L-0001 | codificação UTF-8 inválida |
| NML-L-0002 | identificador inválido |
| NML-L-0003 | string ou escape inválido |
| NML-S-0001 | delimitador não balanceado |
| NML-S-0002 | indentação inválida |
| NML-S-0003 | propriedade sem valor |
| NML-R-0001 | elemento desconhecido |
| NML-R-0002 | propriedade desconhecida |
| NML-R-0003 | região duplicada |
| NML-T-0001 | tipo de propriedade incompatível |
| NML-A-0001 | conteúdo ou atributo inseguro |
| NSL-L-0001 | codificação/token inválido |
| NSL-L-0002 | string/número inválido |
| NSL-S-0001 | bloco ou seletor malformado |
| NSL-S-0002 | declaração sem terminador |
| NSL-S-0003 | indentação inválida |
| NSL-R-0001 | seletor inválido |
| NSL-R-0002 | propriedade desconhecida |
| NSL-T-0001 | valor incompatível com tipo |
| NSL-T-0002 | operação dimensional inválida |
| NSL-C-0001 | conflito de cascata não resolvido |
| NSL-C-0002 | referência fora do escopo |
| INT-0001 | referência NML–NSL não resolvida |
| INT-0002 | incompatibilidade de versão/contrato |
| INT-0003 | expressão Nebula em contexto não autorizado |

## 7. Estabilidade

Códigos, severidades padrão e intervalos devem ser reproduzíveis para a mesma entrada e versão. Mensagens podem ser localizadas; códigos não. Testes verificam código, fase, intervalo e notas, não apenas texto livre.

## 8. Critério de aceitação

Cada regra de erro da gramática e da semântica deve apontar para um código. O corpus positivo/negativo deve verificar os códigos e intervalos esperados. Este contrato é documentação; nenhuma implementação de diagnósticos é declarada concluída.
