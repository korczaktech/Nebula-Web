# Nebula-Web

Ecossistema Web oficial do Nebula. Reúne duas linguagens próprias — **NML (Nebula Markup Language)** e **NSL (Nebula Styling Language)** —, compiladores, infraestrutura compartilhada, Runtime Web, backends, bibliotecas, ferramentas e integrações.

O objetivo é permitir a criação de páginas e aplicações Web usando Nebula e suas linguagens especializadas, preservando interoperabilidade com a plataforma Web existente.

## Princípios do projeto

- Todo código-fonte próprio do ecossistema será escrito em Nebula.
- NML e NSL terão identidades, especificações, gramáticas, compiladores, semânticas e testes independentes.
- A infraestrutura compartilhada integrará as linguagens por contratos públicos, tipos e APIs explícitos, sem fundir suas gramáticas.
- NML será responsável pela estrutura, conteúdo, documentos, componentes e composição.
- NSL será responsável pela apresentação visual, estilos, seletores, propriedades, temas, estados, responsividade e composição visual.
- A saída deverá interoperar com HTML, CSS, DOM e APIs padronizadas dos navegadores.
- WebAssembly será um alvo de compilação e interoperabilidade quando tecnicamente adequado; o projeto não pretende reinventar o padrão.
- Segurança, acessibilidade, desempenho, portabilidade e diagnósticos reproduzíveis são requisitos transversais.
- A integração especial com ZenithOS será exclusiva do navegador Portunus executado dentro do ZenithOS e passará pela ABI/syscalls oficiais, com autorização e mediação do kernel.
- Páginas, documentos NML e estilos NSL não terão acesso arbitrário a syscalls, hardware ou recursos do sistema operacional.

## Linguagens

### NML — Nebula Markup Language

Linguagem própria de marcação para descrever documentos, páginas, conteúdo, componentes, layouts, navegação, eventos e estrutura de aplicações Web.

- Especificação: [NML.md](NML.md)
- Responsabilidade: estrutura e semântica do documento.
- Interoperabilidade: HTML, DOM e APIs padronizadas dos navegadores.
- Estado: requisitos da Fase 1 documentados; implementação e etapas técnicas ainda precisam de evidências próprias.

### NSL — Nebula Styling Language

Linguagem própria de estilos para descrever apresentação visual, seletores, propriedades tipadas, temas, estados, responsividade, animações e composição visual.

- Especificação de requisitos: [NSL.md](NSL.md)
- Decisões técnicas da Fase 2: [especificacao/NSL-FASE-2-DECISOES.md](especificacao/NSL-FASE-2-DECISOES.md)
- Formalização léxica e sintática da Fase 3: [especificacao/NSL-FASE-3-LEXICO-E-SINTAXE.md](especificacao/NSL-FASE-3-LEXICO-E-SINTAXE.md)
- Gramática EBNF inicial: [especificacao/GRAMATICA-NSL-FASE-3.md](especificacao/GRAMATICA-NSL-FASE-3.md)
- Corpus de conformidade planejado: [especificacao/testes/NSL-FASE-3-CORPUS-CONFORMIDADE.md](especificacao/testes/NSL-FASE-3-CORPUS-CONFORMIDADE.md)

## Roadmap geral

O desenvolvimento está dividido em fases com critérios de saída verificáveis. Uma fase só será considerada concluída quando seus entregáveis estiverem documentados e, quando envolver implementação, houver código e testes reproduzíveis. Decisões de requisitos e documentos de planejamento não são evidência de funcionalidade implementada.

| Fase | Objetivo | Principais entregáveis |
|---|---|---|
| **1 — Requisitos e arquitetura** | Definir propósito, escopo, identidade e requisitos de NML, NSL e ecossistema | Especificações de requisitos, arquitetura geral, limites de segurança e contratos iniciais |
| **2 — Decisões técnicas** | Converter requisitos em decisões técnicas detalhadas | Léxico, semântica, tipos, módulos, compilação, Runtime, segurança, ferramentas e critérios de conformidade |
| **3 — Formalização das linguagens** | Fechar sintaxe e semântica normativa de NML e NSL | Tokens, palavras reservadas, gramáticas, precedência, regras de tipos, diagnósticos e corpus de conformidade |
| **4 — Frontends** | Implementar a leitura e análise das linguagens | Lexer, Parser, AST, resolução de símbolos, análise semântica, diagnósticos e testes |
| **5 — IR e compilação** | Criar representação intermediária e pipeline de compilação | IR tipada, validação, otimizações, compilação incremental, cache, paralelismo e backends |
| **6 — Runtime e integração** | Executar aplicações e conectar os componentes do ecossistema | Runtime, APIs de host, gestão de ciclo de vida, integração Nebula, navegadores e backend WebAssembly |
| **7 — Ferramentas e ecossistema** | Disponibilizar uma experiência completa de desenvolvimento | CLI, servidor de desenvolvimento, formatador, linter, debugger, LSP, autocomplete, bibliotecas e sistema de pacotes |
| **8 — Conformidade e versão estável** | Validar qualidade, segurança e compatibilidade antes da estabilização | Suítes de conformidade, testes ponta a ponta, fuzzing, auditorias, benchmarks, documentação final e release estável |

### Fase 1 — Requisitos e arquitetura

**Objetivo:** estabelecer o que o ecossistema e cada linguagem deverão oferecer.

Entregáveis:
- identidade, propósito e escopo independente de NML e NSL;
- separação entre estrutura (NML) e apresentação (NSL);
- requisitos de componentes, estilos, tipos, reatividade, acessibilidade e internacionalização;
- interoperabilidade com a plataforma Web;
- princípios de segurança, portabilidade e integração com Nebula;
- arquitetura geral e limites do adaptador ZenithOS/Portunus.

**Critério de saída:** requisitos consolidados, sem confundir funcionalidades desejadas com funcionalidades já implementadas.

### Fase 2 — Decisões técnicas

**Objetivo:** transformar os requisitos em decisões técnicas suficientemente precisas para orientar a especificação e a implementação.

Entregáveis:
- decisões sobre tokens, gramática, identificadores, Unicode e blocos;
- expressões, operadores, tipos, unidades, funções e módulos;
- seletores, propriedades, cascata, temas, componentes e estados;
- compilador, IR, otimizações e backends;
- Runtime, interoperabilidade, segurança e integração;
- ferramentas, testes, versionamento e critérios de qualidade.

**Estado da NSL:** entrevista técnica concluída com 125 decisões registradas. A consolidação está em [especificacao/NSL-FASE-2-DECISOES.md](especificacao/NSL-FASE-2-DECISOES.md).

**Critério de saída:** decisões rastreáveis e documentadas; qualquer lacuna remanescente deve ser tratada explicitamente na formalização, sem inventar escolhas do usuário.

### Fase 3 — Formalização das linguagens

**Objetivo:** transformar as decisões em especificações normativas, inequívocas e testáveis.

Entregáveis:
- inventário fechado de palavras reservadas e tokens;
- regras lexicais para Unicode, identificadores, strings, números, comentários e espaços;
- delimitadores, blocos, declarações e regras de indentação;
- gramática formal completa;
- precedência e associatividade de operadores;
- semântica de expressões, tipos, módulos, seletores e declarações;
- regras de erro, recuperação e diagnósticos;
- corpus positivo, negativo, semântico e de segurança para cada linguagem.

**Estado da NSL:** em andamento. Há decisões documentadas, uma gramática EBNF inicial e um corpus planejado. A gramática ainda é rascunho técnico e o corpus ainda não foi executado por um lexer/parser de referência.

**Critério de saída:** especificação normativa revisada, sem produções ambíguas ou símbolos indefinidos, com corpus aprovado e executado integralmente por uma implementação de referência.

### Fase 4 — Frontends

**Objetivo:** implementar os compiladores frontends de NML e NSL.

Entregáveis:
- lexer com posições de origem e suporte Unicode;
- Parser baseado na gramática normativa;
- AST pública e tipada;
- resolução de módulos, imports, exports e símbolos;
- análise semântica e validação de tipos;
- diagnósticos estruturados, recuperação de erros e source maps;
- testes unitários, de integração, regressão e conformidade.

**Critério de saída:** cada frontend analisa os programas válidos, rejeita entradas inválidas e passa pelo corpus normativo correspondente.

### Fase 5 — IR e compilação

**Objetivo:** estabelecer uma representação intermediária e um pipeline de compilação robusto.

Entregáveis:
- IR tipada e independente das linguagens de origem;
- validação de invariantes da IR;
- lowering de AST para IR;
- passes de otimização e análise estática;
- compilação incremental e cache determinístico;
- paralelismo seguro quando aplicável;
- geração de HTML/CSS e artefatos WebAssembly quando apropriado;
- source maps e diagnósticos entre as etapas.

**Critério de saída:** pipeline reproduzível, IR validada, otimizações testadas e saídas verificadas por testes de integração e ponta a ponta.

### Fase 6 — Runtime e integração

**Objetivo:** executar as aplicações compiladas e integrar os subsistemas com limites explícitos.

Entregáveis:
- Runtime Web e contratos de ciclo de vida;
- APIs de host e modelo de permissões;
- integração de NML, NSL e Nebula;
- renderização, atualização de estado e eventos;
- interoperabilidade com DOM, navegador e WebAssembly;
- adaptadores de plataforma isolados;
- adaptador Portunus/ZenithOS, com acesso mediado exclusivamente pelo kernel.

**Critério de saída:** aplicações de referência executadas nos ambientes suportados, com isolamento, permissões e comportamento documentados.

### Fase 7 — Ferramentas e ecossistema

**Objetivo:** oferecer ferramentas de desenvolvimento e distribuição.

Entregáveis:
- CLI e gerenciamento de projetos;
- servidor local de desenvolvimento e recarga;
- formatador e linter;
- debugger e inspeção de Runtime;
- Language Server, autocomplete e diagnósticos no editor;
- bibliotecas padrão e componentes;
- gerenciamento de pacotes e dependências;
- documentação de APIs e guias de desenvolvimento.

**Critério de saída:** fluxo de desenvolvimento reproduzível, desde a criação do projeto até compilação, depuração e distribuição.

### Fase 8 — Conformidade e versão estável

**Objetivo:** comprovar que o ecossistema atende aos contratos publicados e pode ser estabilizado.

Entregáveis:
- testes de conformidade completos para NML e NSL;
- testes ponta a ponta e regressão entre versões;
- fuzzing de lexer, parser, IR e Runtime;
- auditorias de segurança e acessibilidade;
- benchmarks de compilação, execução e consumo de recursos;
- testes de compatibilidade entre navegadores e plataformas;
- documentação final, política de versionamento e processo de releases;
- critérios de compatibilidade e suporte da versão estável.

**Critério de saída:** todos os critérios obrigatórios aprovados, problemas críticos resolvidos, resultados reproduzíveis e release formalmente publicada. A designação 1.0 não poderá ser usada apenas porque a documentação foi concluída.

## Estrutura do repositório

- **linguagens/NML**: implementação e artefatos próprios de NML.
- **linguagens/NSL**: implementação e artefatos próprios de NSL.
- **compartilhado**: infraestrutura e contratos compartilhados.
- **compilador**: coordenação do pipeline e serviços comuns de compilação.
- **runtime**: Runtime Web.
- **backend/webassembly**: geração e interoperabilidade WebAssembly.
- **integracoes**: Nebula, navegadores, Portunus e ZenithOS.
- **ferramentas**: CLI, formatador, linter, debugger e Language Server.
- **bibliotecas**: bibliotecas padrão e componentes.
- **testes**: testes unitários, integração, conformidade, regressão e ponta a ponta.
- **especificacao**: arquitetura, decisões, gramáticas, contratos e roadmaps.

Diretórios reservados representam organização planejada e não comprovam que seus componentes já estejam implementados.

## Estado atual do projeto

O repositório possui estrutura arquitetural inicial e documentação de requisitos de NML e NSL. Para a NSL, a entrevista de decisões técnicas da Fase 2 foi concluída e a Fase 3 está em formalização inicial.

| Área | Estado documentado |
|---|---|
| Estrutura inicial do repositório | Criada |
| Requisitos NML — Fase 1 | Documentados |
| Requisitos NSL — Fase 1 | Documentados |
| Decisões técnicas NSL — Fase 2 | Entrevista concluída e decisões registradas |
| Formalização NSL — Fase 3 | Em andamento; gramática e corpus ainda não normativos |
| Frontends NML e NSL — Fase 4 | Não declarados implementados |
| IR e compilação — Fase 5 | Não declarados implementados |
| Runtime e integração — Fase 6 | Não declarados implementados |
| Ferramentas e ecossistema — Fase 7 | Não declarados implementados |
| Conformidade e release — Fase 8 | Não iniciadas como etapa de estabilização |

**Regra de status:** somente código presente no repositório, testes executados e evidências reproduzíveis autorizam marcar um subsistema como implementado ou concluído.

## Documentação principal

- [Especificação NML](NML.md)
- [Especificação NSL](NSL.md)
- [Arquitetura geral](especificacao/arquitetura-geral.md)
- [Roadmap técnico](especificacao/roadmap.md)
- [Decisões da Fase 2 da NSL](especificacao/NSL-FASE-2-DECISOES.md)
- [Formalização léxica e sintática da Fase 3 da NSL](especificacao/NSL-FASE-3-LEXICO-E-SINTAXE.md)
- [Gramática EBNF inicial da NSL](especificacao/GRAMATICA-NSL-FASE-3.md)
- [Corpus de conformidade planejado da NSL](especificacao/testes/NSL-FASE-3-CORPUS-CONFORMIDADE.md)
- [Léxico e sintaxe da Fase 3 da NML](especificacao/NML-FASE-3-LEXICO-E-SINTAXE.md)
- [Gramática EBNF da NML](especificacao/GRAMATICA-NML-FASE-3.md)
- [Modelo semântico e AST da NML](especificacao/NML-MODELO-SEMANTICO-E-AST.md)
- [Modelo semântico e AST da NSL](especificacao/NSL-MODELO-SEMANTICO-E-AST.md)
- [Contratos NML–NSL–Nebula](especificacao/CONTRATOS-NML-NSL-NEBULA.md)
- [Diagnósticos e recuperação](especificacao/DIAGNOSTICOS-E-RECUPERACAO.md)
- [Corpus de integração NML–NSL–Nebula](especificacao/testes/INTEGRACAO-NML-NSL-NEBULA-CORPUS.md)
- [Contribuição](CONTRIBUTING.md)

## Contribuição

Consulte [CONTRIBUTING.md](CONTRIBUTING.md) antes de alterar arquitetura, especificações ou código. Toda funcionalidade deverá incluir testes automatizados e documentação. Não declare suporte, implementação ou conformidade sem evidências reproduzíveis.


### Decisão de sintaxe inicial de NML e NSL

As decisões iniciais aprovadas para regiões opcionais e aninhamento em NML, declaração explícita de texto e seletores de classe/elemento em NSL estão documentadas em [0003 — Sintaxe inicial de NML e NSL](especificacao/decisoes-arquiteturais/0003-sintaxe-inicial-nml-nsl.md).


## Formalização das linguagens — Fase 3

A primeira consolidação documental de NML e a revisão da sintaxe NSL estão em draft técnico. Os documentos definem léxico, gramática, AST, semântica, diagnósticos, corpus esperado e contratos de integração; não representam implementação nem testes executados.

### NML
- [Léxico e sintaxe](especificacao/NML-FASE-3-LEXICO-E-SINTAXE.md)
- [Gramática EBNF](especificacao/GRAMATICA-NML-FASE-3.md)
- [Modelo semântico e AST](especificacao/NML-MODELO-SEMANTICO-E-AST.md)
- [Corpus de conformidade](especificacao/testes/NML-FASE-3-CORPUS-CONFORMIDADE.md)

### NSL
- [Léxico e sintaxe](especificacao/NSL-FASE-3-LEXICO-E-SINTAXE.md)
- [Gramática EBNF](especificacao/GRAMATICA-NSL-FASE-3.md)
- [Modelo semântico e AST](especificacao/NSL-MODELO-SEMANTICO-E-AST.md)
- [Corpus de conformidade](especificacao/testes/NSL-FASE-3-CORPUS-CONFORMIDADE.md)

### Contratos compartilhados
- [Contratos NML–NSL–Nebula](especificacao/CONTRATOS-NML-NSL-NEBULA.md)
- [Diagnósticos e recuperação](especificacao/DIAGNOSTICOS-E-RECUPERACAO.md)

**Regra de status:** documentos em draft não autorizam marcar a Fase 3 como aprovada; isso exige revisão normativa. A implementação em Nebula e a execução dos corpora são etapas posteriores.
