# NSL — Modelo semântico, tipos, seletores e AST

**Versão:** 0.2.0-draft  
**Estado:** consolidação técnica proposta

## 1. Responsabilidade

NSL descreve apresentação visual. Não cria estrutura documental e não substitui NML. Cada arquivo NSL possui lexer, parser, AST, análise semântica e diagnósticos próprios.

## 2. Formas básicas aprovadas

Seletor de classe, com ponto:

§§§nsl
.nome: {
    cor: "#ffffff";
}
§§§

Seletor de elemento, sem ponto:

§§§nsl
nome: {
    cor: "#ffffff";
}
§§§

§.nome§ seleciona elementos associados à classe §nome§; §nome§ seleciona elementos pelo nome/tipo §nome§.

## 3. Extensões de seletores propostas

Para completar o modelo técnico, ficam propostas as formas:
- §#identificador§: seletor de id único;
- §[atributo]§: presença de atributo;
- §[atributo="valor"]§: igualdade de atributo;
- §A B§: descendente;
- §A > B§: filho direto;
- §A + B§: irmão adjacente;
- §A ~ B§: irmão posterior;
- §:estado§: estado/pseudoclasse;
- §::parte§: parte/pseudoelemento;
- §,§: grupo de seletores.

Essas extensões são propostas normativas técnicas sujeitas ao corpus antes da estabilização. Não alteram o significado aprovado das duas formas básicas.

## 4. Declarações de estilo

A forma declarativa usa dois-pontos e ponto e vírgula:

§§§nsl
.nome: {
    cor: "#ffffff";
    tamanhoFonte: 16px;
}
§§§

- Declarações simples terminam em §;§.
- Propriedades duplicadas no mesmo bloco são erro, exceto as acumuláveis.
- Propriedades desconhecidas são erro, salvo namespace/extensão importado.
- Valores são validados pelo tipo e domínio da propriedade.
- Regras aninhadas são permitidas somente quando a produção gramatical declarar seu contexto.
- Marcador de prioridade não contorna escopo nem segurança.

## 5. Especificidade

Cada seletor recebe tupla §(a,b,c)§:
- §a§: quantidade de seletores de id;
- §b§: quantidade de classes, atributos e estados;
- §c§: quantidade de seletores de elemento e partes.

O maior valor lexicográfico vence. Universal e combinadores não acrescentam especificidade. Em grupos, cada ramo mantém sua especificidade. Pseudoclasses funcionais seguem a regra de seu argumento, sem dupla contagem.

## 6. Cascata

Ordem de resolução, da menor para a maior precedência:
1. origem e camada;
2. especificidade;
3. proximidade do escopo;
4. prioridade declarada;
5. ordem de origem como desempate final.

A cascata considera origem (host/agente, biblioteca, autor), camada, escopo, condição de ambiente e prioridade. A regra é determinística e explicável pelo inspetor. Camadas são declaradas no módulo; camadas não declaradas seguem ordem de primeira aparição. A tabela de prioridade normal/importante deverá preservar compatibilidade documentada com CSS quando houver equivalência.

## 7. Herança e valores

Cada propriedade declara modo: herdada, não herdada, herdada por escopo ou calculada pelo runtime.

Valores são literal tipado, referência a token, função de valor, expressão permitida ou palavra-chave semântica. Ausência, inicial, herdado e calculado são estados diferentes. §nulo§ não é reset universal.

## 8. Sistema de tipos

Tipos-base: texto, inteiro, decimal, lógico, cor, medida, duração, ângulo, percentual, resolução, lista, tupla, função, referência a token e união tipada.

- Medidas carregam dimensão e unidade.
- Operações dimensionais incompatíveis são erro.
- Conversões dependentes de contexto exigem função explícita.
- Cores têm espaço de cor, perfil e canais tipados.
- Propriedades declaram domínio, faixa, valor inicial, herança, animação e compatibilidade.
- Tokens têm tipo declarado ou inferido deterministicamente.
- Valores dinâmicos são validados estaticamente sempre que possível e em runtime quando dependem de dados.

## 9. AST pública

| Nó | Conteúdo |
|---|---|
| NslModule | módulo, imports, declarações, versão |
| NslImport | caminho, alias, símbolos |
| NslRule | seletor, bloco, origem |
| NslSelector | árvore tipada |
| NslSelectorElement | nome de elemento |
| NslSelectorClass | nome de classe |
| NslSelectorId | identificador |
| NslSelectorAttribute | atributo e operador |
| NslSelectorRelation | relação entre seletores |
| NslSelectorState | estado/pseudoclasse |
| NslDeclaration | propriedade, valor e prioridade |
| NslValue | literal, medida, cor, lista, função, token |
| NslTokenDeclaration | token tipado |
| NslTheme | tema e substituições |
| NslCondition | consulta/condição |
| NslAnimation | quadros e temporização |
| NslErrorNode | nó sintético de recuperação |

Todos os nós carregam localização e preservam grafia original. AST inválida é parcial e não pode ser usada como saída semântica final.

## 10. Escopos e temas

- Escopos global, módulo, componente e local são distintos.
- Estilos locais não vazam para outros componentes salvo exportação explícita.
- Temas substituem tokens por identidade tipada, sem reescrever AST de regras.
- Tokens não resolvidos são erro, exceto referências opcionais com fallback tipado.
- Imports e namespaces têm resolução determinística e detecção de ciclos.

## 11. Integração CSS

A saída CSS preserva semântica NSL quando houver mapeamento fiel. Recursos sem equivalência: transformação para CSS + runtime documentado, fallback explícito, aviso de perda ou erro quando a perda afetar segurança/resultado essencial. Nenhum recurso declara equivalência sem teste diferencial.

## 12. Segurança

Seletores não atravessam escopos isolados sem contrato. Valores não confiáveis não geram CSS executável ou URLs perigosas. Recursos externos respeitam políticas do navegador. NSL não concede acesso a hardware/syscalls; integração ZenithOS só existe no adaptador autorizado Portunus/kernel.
