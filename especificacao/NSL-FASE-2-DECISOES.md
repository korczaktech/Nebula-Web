# NSL — Consolidação das decisões da Fase 2

**Estado:** decisões aprovadas; especificação normativa em elaboração  
**Versão:** 0.2.0-draft  
**Idioma:** português brasileiro  
**Padrão de nomenclatura:** `camelCase`  
**Escopo:** 125 decisões técnicas

> Este documento registra decisões de requisitos. Não constitui prova de implementação. Nenhum recurso descrito deve ser considerado implementado sem código, testes e evidências verificáveis.

## 1. Resultado da entrevista

A entrevista técnica da Fase 2 foi concluída com 125 respostas. Todas as perguntas receberam a opção 4. Na decisão sobre nomenclatura, a opção 4 foi complementada explicitamente por `camelCase` como convenção padrão. O sistema continuará configurável por categoria, com convenções oficiais e validação automática.

As escolhas representam a direção arquitetural desejada: uma linguagem própria, formal, extensível, tipada, segura, interoperável e profundamente integrada ao ecossistema Nebula-Web.

## 2. Sintaxe, léxico e semântica

NSL deverá possuir:
- estrutura formal de arquivos com metadados, módulos, namespaces, declarações e dependências;
- blocos delimitados por uma construção sintática própria, com aninhamento e escopos formais;
- seletores com identificadores tipados, relações, operadores e composição;
- declarações tipadas, expressões e validação semântica;
- separadores contextuais sem ambiguidades;
- comentários de linha, bloco, documentação e metadados;
- identificadores Unicode com normalização definida e proteção contra confusáveis;
- strings tipadas, multilinha, interpolação, escapes e templates;
- literais numéricos tipados, precisão definida, notações e unidades;
- literais reservados em português para booleanos e ausência de valor;
- variáveis com tipos, inferência, constantes, mutabilidade, escopos e reatividade;
- importação e exportação seletivas, aliases, namespaces hierárquicos e resolução determinística;
- whitespace e indentação formalmente especificados;
- recuperação sintática e diagnósticos múltiplos com localização, contexto, sugestões e causa provável.

## 3. Expressões, tipos e valores

O modelo semântico deverá incluir:
- operadores matemáticos, lógicos e relacionais com precedência e associatividade normativas;
- sobrecarga de operadores apenas quando definida por regras de tipos seguras;
- expressões aninhadas, AST, inferência, avaliação e otimização;
- funções com parâmetros, retornos, genéricos, sobrecarga e inferência;
- expressões reativas com grafo de dependências, memoização e invalidação;
- sistema dimensional de unidades, conversões e validação dimensional;
- cores com espaços de cor, perfis, transparência, conversões e operações;
- tipos primitivos, compostos, personalizados e genéricos;
- conversões implícitas restritas e conversões explícitas verificáveis.

A gramática concreta dos operadores, palavras reservadas e delimitadores ainda precisa ser ratificada em documento normativo. Não se deve inferir grafia de tokens a partir dos nomes conceituais deste arquivo.

## 4. Seletores, propriedades e apresentação

O sistema visual deverá incluir:
- catálogo amplo de propriedades nativas tipadas e extensíveis;
- propriedades personalizadas com namespaces, validação e herança configurável;
- seletores de elementos, identificadores, classes, atributos, relações, estados e composição;
- estados nativos, personalizados e reativos integrados ao NML;
- condições compostas, consultas de contexto e validação estática;
- algoritmo próprio, formal e determinístico de especificidade;
- herança controlável por propriedade e por escopo;
- cascata com camadas, prioridades, origem, escopo, condições e resolução determinística;
- resolução de conflitos rastreável e explicável;
- validação estática e dinâmica de seletores e propriedades.

A linguagem deverá fornecer layout, responsividade, animações, transformações 2D/3D, efeitos, tipografia, temas, design tokens, componentes, pseudoelementos e consultas de ambiente. Os recursos deverão possuir tipagem, composição, fallbacks e regras de compatibilidade explícitas.

## 5. Compilador e toolchain

A arquitetura desejada compreende:
1. Lexer Unicode e estados léxicos;
2. Parser com gramática formal e recuperação;
3. AST tipada com metadados e localização de origem;
4. análise semântica de símbolos, escopos, tipos e dependências;
5. IR independente de backend, validável e extensível;
6. otimizações com preservação semântica;
7. backends múltiplos e interoperáveis;
8. compilação incremental por dependências;
9. cache em camadas com invalidação;
10. paralelismo determinístico;
11. source maps e rastreamento de origem;
12. diagnósticos estruturados.

A implementação pretendida é em Nebula. O compilador deverá ter limites de recursos, entradas validadas e isolamento de plugins. A saída CSS deverá preservar semântica sempre que possível; divergências deverão gerar fallback documentado, aviso ou erro.

## 6. Runtime, integração e plataformas

NSL deverá prever:
- runtime modular para reatividade, dependências e estilos dinâmicos;
- execução híbrida com CSS nativo e recursos do runtime quando necessários;
- matriz de compatibilidade de navegadores, detecção de capacidades e fallbacks;
- interoperabilidade tipada e controlada com JavaScript e WebAssembly;
- integração com Web Components, Custom Elements e Shadow DOM;
- renderizadores intercambiáveis e execução multiplataforma;
- APIs versionadas, tipadas e documentadas;
- comunicação entre processos por mensagens e contratos autorizados;
- gerenciamento de recursos, limites, observabilidade e ciclo de vida.

### 6.1 NML e Nebula

NML continua responsável por estrutura, conteúdo e composição; NSL, por apresentação visual. A comunicação entre ambas deverá usar contratos tipados para componentes, propriedades, estados, eventos, slots e dependências. Compilação e validação cruzadas deverão ocorrer sem fundir as gramáticas.

### 6.2 ZenithOS e Portunus

A integração com ZenithOS deverá respeitar a ABI, permissões e mediação do kernel. Recursos especiais só poderão ser usados no contexto autorizado de Portunus executando dentro do ZenithOS. NSL não concederá acesso direto a hardware, memória privilegiada ou syscalls arbitrárias. O acesso será mediado por APIs e syscalls explicitamente autorizadas pelo kernel.

## 7. Segurança, qualidade e ferramentas

Requisitos aprovados:
- sanitização, políticas de segurança, análise de risco e integração com CSP;
- isolamento e permissões de plugins, limites de recursos e validação de entradas;
- testes unitários, integração, conformidade, regressão, fuzzing e desempenho;
- suíte formal de conformidade com casos positivos, negativos e testes diferenciais;
- documentação normativa, técnica, tutorial, referências, exemplos e API;
- LSP, autocomplete, navegação, refatoração e diagnósticos;
- formatador determinístico, linter, correções seguras e depurador completo;
- telemetria configurável, métricas, tracing e profiling com respeito à privacidade;
- CI/CD, builds reproduzíveis, hashes, proveniência, assinaturas e auditoria.

## 8. Projetos, pacotes e governança

O ecossistema deverá definir:
- estrutura de projeto formal, configurável e validável;
- configuração tipada e hierárquica com perfis e precedência;
- ambientes reproduzíveis;
- gerenciador de dependências com versões, integridade, resolução determinística, cache e auditoria;
- registro de pacotes com namespaces, permissões, versões e auditoria;
- distribuição e assinatura de artefatos;
- propostas formais para evolução da gramática;
- versionamento semântico, compatibilidade, migrações e depreciação;
- governança pública, revisão técnica e histórico de decisões.

## 9. Critérios de aceitação da versão 1.0

NSL 1.0 só poderá ser declarada estável quando:
1. a especificação normativa estiver fechada e versionada;
2. a gramática e a semântica forem inequívocas;
3. compilador, runtime e backends exigidos estiverem implementados;
4. a suíte de conformidade passar integralmente nos ambientes suportados;
5. integrações NML/Nebula possuírem contratos e testes;
6. segurança, acessibilidade, compatibilidade e desempenho tiverem critérios mensuráveis e evidências;
7. documentação, ferramentas e política de evolução estiverem publicadas;
8. não houver pendências críticas conhecidas.

## 10. Pendências de especificação

As decisões de alto nível não determinam, por si só, cada token, produção gramatical ou algoritmo. Antes de chamar a gramática de normativa, ainda devem ser especificados e revisados:
- grafia exata de delimitadores, operadores e palavras reservadas;
- gramática EBNF completa e precedência de todas as expressões;
- modelo Unicode, normalização e política de confusáveis;
- tabela de tipos, regras de inferência e conversões;
- algoritmo numérico da especificidade e ordenação da cascata;
- catálogo normativo de propriedades, valores e unidades;
- modelo de escopo, módulos, importação e exportação;
- contratos ABI/API de runtime, NML, Nebula e plataformas;
- formatos de IR, source maps e artefatos;
- matriz de compatibilidade e critérios quantitativos de desempenho.

Essas pendências devem ser resolvidas em revisões técnicas rastreáveis. Exemplos neste estágio são ilustrativos e não podem ser tratados como sintaxe aprovada.
