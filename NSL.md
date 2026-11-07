# NSL — Nebula Styling Language
## Especificação de requisitos e arquitetura da linguagem

**Documento:** `NSL.md`  
**Projeto:** Nebula-Web  
**Estado:** Especificação de requisitos consolidada — Fase 1  
**Versão do documento:** 0.1.0  
**Idioma:** Português brasileiro  
**Implementação pretendida:** Nebula  
**Escopo da entrevista:** 125 decisões

---

## Estado da especificação

A Fase 1 consolidou os requisitos de produto e arquitetura. A entrevista técnica da Fase 2 também foi concluída, com 125 decisões registradas. A consolidação está em [`especificacao/NSL-FASE-2-DECISOES.md`](especificacao/NSL-FASE-2-DECISOES.md). A base de gramática está em [`especificacao/GRAMATICA-NSL-RASCUNHO.md`](especificacao/GRAMATICA-NSL-RASCUNHO.md) e permanece explicitamente não normativa até que os tokens e regras concretas sejam aprovados.

As decisões representam requisitos, não funcionalidades implementadas. A versão 1.0 só poderá ser declarada estável após implementação verificável, testes de conformidade e atendimento aos critérios documentados.

---

## 1. Objetivo

A **Nebula Styling Language (NSL)** é a linguagem própria de apresentação visual do ecossistema Nebula-Web. Seu propósito é descrever, validar, compor, otimizar e aplicar estilos a documentos, elementos e componentes produzidos por NML, além de elementos HTML e componentes Web interoperáveis.

NSL não deverá ser apenas uma tradução de CSS. Terá sintaxe, gramática, semântica, sistema de tipos, compilador, ferramentas e ecossistema próprios, mantendo interoperabilidade com CSS e com a plataforma Web existente.

NSL será uma linguagem independente de apresentação visual, com integração profunda — mas formalmente delimitada — com Nebula e NML.

Este documento registra requisitos escolhidos na entrevista. **Ele não afirma que os recursos descritos já estejam implementados.** Uma capacidade só poderá ser considerada concluída quando existir implementação verificável e testes correspondentes.

## 2. Princípios fundamentais

### 2.1 Identidade própria

NSL terá identidade sintática e semântica própria. Conceitos familiares de CSS poderão ser aproveitados quando úteis, mas não obrigam a linguagem a reproduzir sua sintaxe ou todas as suas limitações.

### 2.2 Português como padrão

Palavras-chave e construções reservadas deverão priorizar português brasileiro, com nomenclatura técnica consistente e alinhada ao Nebula. As regras exatas de identificadores, palavras reservadas, acentuação e convenções serão formalizadas na gramática.

### 2.3 Independência com integração

NSL deverá possuir especificação, compilador e ferramentas próprios. A integração com Nebula e NML ocorrerá por interfaces, contratos, tipos e APIs documentados, evitando acoplamento implícito entre gramáticas.

### 2.4 Interoperabilidade Web

A implementação deverá produzir saídas interoperáveis com CSS e com os mecanismos visuais padronizados dos navegadores. A interoperabilidade deverá preservar a semântica sempre que possível; quando houver diferenças, o compilador deverá emitir diagnóstico, aplicar fallback documentado ou rejeitar a conversão insegura.

### 2.5 Segurança por padrão

NSL não deverá contornar sandbox, política de mesma origem, CSP, CORS, permissões ou outras restrições do navegador. Estilos e recursos externos deverão respeitar políticas de origem, integridade e permissões.

### 2.6 Implementação e comprovação

A implementação pretendida será escrita em Nebula. Cada subsistema deverá possuir especificação técnica, implementação, testes automatizados e critérios de aceitação. Requisitos não implementados deverão permanecer identificados como pendentes.

## 3. Modelo geral da linguagem

NSL deverá oferecer um sistema completo de apresentação visual, incluindo:

- seletores, relações, estados e escopos;
- propriedades nativas, personalizadas e tipadas;
- valores, expressões, unidades e funções;
- cascata, especificidade, herança e resolução determinística de conflitos;
- temas, tokens, variáveis e composição;
- layouts, posicionamento e responsividade;
- tipografia, cores, geometria e efeitos;
- animações, transições e interações visuais;
- integração reativa com dados, estados e componentes;
- módulos, bibliotecas, plugins e extensões;
- análise estática, otimização, compilação e geração de saídas;
- diagnósticos, depuração, testes e ferramentas de IDE.

A sintaxe concreta de cada recurso deverá ser definida em especificação gramatical própria. Os nomes usados neste documento são descrições conceituais, não palavras-chave normativas.

## 4. Seletores, correspondência e cascata

### 4.1 Sistema de seletores

O sistema deverá suportar seletores de elementos, componentes, classes ou identificadores equivalentes, relações hierárquicas, estados, propriedades, slots e contextos. A gramática deverá definir precisamente cada categoria e evitar ambiguidades.

### 4.2 Seletores de componentes

Deverá ser possível selecionar componentes por identidade, propriedades, variantes, estados internos e externos, slots, composição e escopo. A seleção de detalhes internos de um componente deverá obedecer às fronteiras de encapsulamento e às partes explicitamente expostas.

### 4.3 Estados

O sistema deverá reconhecer estados de interação, aplicação, componente, dados e condições. Estados personalizados deverão ser tipados e integráveis ao sistema reativo Nebula. A disponibilidade de estados dependerá do contexto e das capacidades da plataforma.

### 4.4 Especificidade

A especificidade será calculada por regras formais, com níveis definidos, prioridades configuráveis e consideração de escopos. O algoritmo deverá ser determinístico e explicável pelas ferramentas.

### 4.5 Cascata

A cascata deverá considerar, conforme regras normativas, origem dos estilos, camadas, prioridades, escopos, especificidade, condições e ordem de declaração. A especificação deverá definir uma ordenação total ou um mecanismo de desempate determinístico para todo conflito válido.

### 4.6 Diagnóstico de conflitos

O compilador e as ferramentas deverão explicar qual declaração venceu, quais declarações foram superadas e quais fatores determinaram o resultado. A explicação deverá rastrear a origem até o arquivo, módulo e regra correspondentes.

## 5. Propriedades, valores e sistema de tipos

### 5.1 Propriedades nativas

NSL deverá possuir catálogo organizado de propriedades nativas, agrupadas por categorias como dimensões, espaçamento, bordas, cores, tipografia, layout, posicionamento, efeitos, animação, geometria, interação e acessibilidade.

Cada propriedade deverá documentar:
- nome e finalidade;
- tipo e domínio de valores;
- valor inicial ou comportamento padrão;
- possibilidade de herança;
- comportamento de cálculo;
- aplicabilidade a elementos e componentes;
- efeitos na renderização;
- compatibilidade e fallback;
- requisitos de acessibilidade, quando aplicáveis.

### 5.2 Propriedades personalizadas

Autores e bibliotecas poderão declarar propriedades próprias. O sistema deverá suportar tipos, valores padrão, restrições, herança, dependências, validação e reatividade. O registro e a visibilidade dessas propriedades deverão ser controlados por escopo e módulo.

### 5.3 Sistema de tipos

O sistema deverá incluir tipos estáticos, inferência, genéricos, restrições e tipos compostos. O compilador deverá verificar compatibilidade de valores, expressões, unidades, funções, propriedades e parâmetros de módulos.

A especificação deverá distinguir erros detectáveis em compilação daqueles que dependem de dados ou capacidades disponíveis apenas em execução.

### 5.4 Valores e expressões

Valores poderão ser literais, calculados, dinâmicos, referenciados ou reativos. Expressões deverão ser tipadas e possuir regras formais de avaliação, precedência, associatividade, efeitos permitidos e dependências.

A avaliação estática deverá ocorrer quando o resultado puder ser determinado com segurança em compilação. Valores dependentes de estado ou ambiente poderão ser avaliados em execução, com atualização incremental.

### 5.5 Funções nativas

A biblioteca deverá oferecer funções matemáticas, geométricas, tipográficas, cromáticas, condicionais, de transformação e reatividade. Cada função deverá declarar assinatura, tipos, unidades, domínio, comportamento em casos extremos e possibilidade de avaliação em compilação.

### 5.6 Unidades

O sistema deverá suportar unidades absolutas, relativas, contextuais, físicas e personalizadas, conversões tipadas e validação dimensional. Operações incompatíveis deverão produzir diagnósticos. A semântica das unidades deverá interoperar com unidades CSS quando houver correspondência válida.

## 6. Cores, tipografia e recursos visuais

### 6.1 Cores

O sistema deverá contemplar múltiplos espaços de cores, transparência, gradientes, mistura, funções, perfis e cores dinâmicas. Conversões deverão respeitar a semântica dos espaços de cor e declarar perdas quando uma saída não puder representar fielmente o valor original.

### 6.2 Tipografia

Deverá incluir famílias, fontes variáveis e personalizadas, carregamento dinâmico, fallback, tipografia responsiva, regras de texto, espaçamento e composição. O carregamento de fontes externas deverá respeitar políticas de origem, integridade, cache e segurança.

### 6.3 Imagens e recursos

O sistema deverá estilizar e compor imagens, SVG, vetores, máscaras, fundos compostos e recursos externos. Deverá oferecer carregamento otimizado, sob demanda, cache, integridade e fallbacks.

### 6.4 Bordas, formas e geometria

Deverá contemplar bordas tipadas, raios, contornos, formas personalizadas, recortes, caminhos vetoriais, polígonos, curvas e operações geométricas. Recursos 2D e 3D deverão ser expressos por propriedades e funções tipadas.

### 6.5 Efeitos e filtros

O sistema deverá oferecer filtros, sombras, máscaras, recortes, transparências, mistura de camadas e efeitos personalizados. Pipelines de efeitos deverão ter composição explícita, parâmetros tipados, validação e otimização.

### 6.6 Gráficos e visualização de dados

NSL deverá fornecer recursos visuais para gráficos, escalas, eixos, legendas, temas e animações, integráveis a dados Nebula. A semântica dos dados e a lógica de cálculo não deverão ser confundidas com a apresentação visual.

### 6.7 Canvas, SVG e 3D

A integração deverá abranger estilização, geometria, transformações, filtros, animações e interação com SVG e Canvas, respeitando as APIs do navegador. Recursos 3D deverão contemplar perspectiva, transformações, composição e integração com tecnologias gráficas disponíveis, sem prometer suporte inexistente no ambiente.

### 6.8 Aceleração gráfica

O runtime deverá detectar capacidades e selecionar estratégias compatíveis, com fallback, segurança, gerenciamento de recursos e otimização. A aceleração por hardware será condicional à disponibilidade e às decisões do navegador e do dispositivo.

## 7. Box model, layout e posicionamento

### 7.1 Modelo de dimensões

O modelo deverá oferecer dimensões, margens, preenchimentos, bordas, limites, proporções, restrições e modelos de dimensionamento configuráveis. A semântica deverá ser precisa e interoperável com os modelos Web de destino.

### 7.2 Layouts

Deverá incluir fluxo, Flexbox, Grid, layouts adaptativos e mecanismos próprios de composição. Regras de dimensionamento, alinhamento, distribuição, áreas, gaps e comportamento em conteúdo variável deverão ser formalizadas.

### 7.3 Posicionamento e camadas

Deverá suportar posicionamento normal, relativo, absoluto, fixo e aderente, âncoras, camadas, contextos de empilhamento, hierarquia, isolamento e prioridades. A ordem de composição deverá ser determinística.

### 7.4 Rolagem

Deverá contemplar áreas de rolagem, encaixe, âncoras, comportamentos configuráveis, animações e acessibilidade. O sistema deverá respeitar preferências do usuário e capacidades do dispositivo.

### 7.5 Cursores e indicadores

Deverá oferecer cursores e indicadores contextuais, estados de interação e adaptação a diferentes dispositivos de entrada. Indicadores visuais não poderão ser o único meio de transmitir informação essencial.

## 8. Responsividade e condições ambientais

### 8.1 Responsividade

O sistema deverá responder a viewport, dispositivo, contêiner, orientação, capacidades e condições dinâmicas. Deverá suportar breakpoints e condições personalizadas, com avaliação previsível e atualização reativa.

### 8.2 Consultas

Deverá oferecer consultas de mídia, recursos, preferências, tamanho de contêiner, estado, contexto e condições combinadas. A gramática deverá distinguir consultas estáticas, avaliadas em compilação, de consultas dependentes do ambiente.

### 8.3 Dispositivos de entrada

Deverá reconhecer capacidades de mouse, toque, caneta e teclado, bem como características relevantes de interação. A detecção deverá utilizar mecanismos da plataforma e não inferir características não fornecidas pelo navegador.

### 8.4 Preferências do usuário

Deverá contemplar temas, movimento, contraste, transparência, cores, densidade visual e preferências personalizadas. Preferências de acessibilidade do usuário terão precedência conforme regras normativas, evitando que estilos da aplicação as neutralizem indevidamente.

### 8.5 Internacionalização visual

Deverá suportar direção de escrita, idiomas, sistemas tipográficos, layouts culturais, propriedades lógicas e adaptação contextual. A apresentação deverá funcionar com diferentes sistemas de escrita e direções.

### 8.6 Densidade visual

Deverá suportar densidade global, contextual, adaptativa, herdável e configurável pelo usuário, com regras de precedência explícitas.

## 9. Animações, transições e gestos

### 9.1 Animações

O sistema deverá suportar sequências, animações compostas, controle temporal, estados, eventos, animações dinâmicas e integração com Nebula. A semântica de execução, repetição, pausa, reversão, interrupção e sincronização deverá ser definida.

### 9.2 Transições

Deverá oferecer duração, atraso, curvas personalizadas, transições compostas, interrupções, reversões, estados e controle programático.

### 9.3 Transformações

Deverá suportar transformações 2D e 3D, matrizes, perspectiva, composição e origem configurável, com integração às animações.

### 9.4 Gestos e eventos

O sistema deverá permitir reconhecimento e composição de gestos, estados e integração com dispositivos de entrada. Eventos deverão ser tipados e integrados a NML e Nebula. NSL definirá os efeitos visuais associados, não substituirá a lógica geral de eventos da aplicação.

### 9.5 Movimento reduzido

Animações e transições deverão respeitar preferências de movimento reduzido. A especificação deverá definir comportamento alternativo para movimentos que possam causar desconforto ou prejudicar a compreensão.

## 10. Temas, tokens e reutilização

### 10.1 Temas

O sistema deverá suportar temas dinâmicos, hierarquias, variantes, herança, composição, troca reativa e integração com Nebula. Temas deverão poder ser aplicados globalmente, por módulo, componente ou contexto, conforme regras explícitas.

### 10.2 Variáveis e tokens

Variáveis deverão ser tipadas, possuir escopo, dependências, expressões, reatividade, validação e integração com Nebula. Tokens de design deverão permitir centralizar decisões visuais e distribuí-las entre bibliotecas e projetos.

### 10.3 Reutilização

Deverá oferecer módulos, composição, herança controlada, mixins, parâmetros, genéricos e reutilização entre projetos. O sistema deverá evitar duplicação e preservar rastreabilidade da origem das declarações.

### 10.4 Escopos e encapsulamento

Deverá suportar escopos globais, locais, hierárquicos, encapsulados, herdáveis e configuráveis. Fronteiras de componente deverão impedir vazamentos não autorizados, mantendo mecanismos explícitos para expor estilos ou partes.

### 10.5 Composição e mixins

A composição deverá ser declarativa, hierárquica, condicional, parametrizada e validada pelo compilador. Mixins deverão suportar parâmetros tipados, valores padrão, genéricos e composição.

### 10.6 Condições e geração

A linguagem deverá suportar condições tipadas, expressões, estados, dados, ambiente, consultas, iteração, filtros, geração estática e dinâmica e expansão em compilação. A geração deverá ser determinística para entradas e configurações equivalentes.

## 11. Componentes, NML e Web Components

### 11.1 Integração NML

NSL deverá integrar-se profundamente à árvore de elementos, componentes, propriedades, estados, eventos, composição, escopos e atualização reativa de NML. As duas linguagens permanecerão especificadas e compiladas por subsistemas próprios, com contratos de integração.

### 11.2 Componentes visuais

Deverá oferecer contratos visuais, propriedades tipadas, variantes, estados, slots, composição, herança controlada e bibliotecas distribuíveis. A API visual de um componente deverá declarar quais propriedades e partes são públicas.

### 11.3 Elementos HTML e personalizados

NSL deverá reconhecer, estilizar e estender elementos HTML e componentes personalizados. A compatibilidade deverá incluir propriedades, estados, slots e relações relevantes.

### 11.4 Web Components e Shadow DOM

A integração deverá abranger Custom Elements, Shadow DOM, slots, partes exportadas, propriedades tipadas, encapsulamento e composição. O acesso a detalhes internos deverá respeitar as fronteiras do Shadow DOM e as interfaces públicas expostas.

### 11.5 Isolamento

O sistema deverá fornecer encapsulamento, escopos, fronteiras de herança, controle de acesso e isolamento entre estilos internos e externos. Exceções deverão ser explícitas e verificáveis.

## 12. Integração com Nebula e dados

### 12.1 Funções e módulos

NSL deverá integrar funções, módulos, tipos, expressões, eventos e reatividade de Nebula, respeitando permissões e limites de execução do navegador. A especificação deverá definir quais funções podem ser avaliadas em compilação e quais exigem runtime.

### 12.2 Dados dinâmicos

Deverá oferecer vinculação tipada, dependências, transformações, atualização incremental e estados assíncronos. A atualização visual deverá observar mudanças relevantes sem recalcular partes independentes.

### 12.3 Runtime Web

A integração deverá cobrir ciclo de vida, renderização, estado, eventos, reatividade, atualização incremental e gerenciamento de recursos. O contrato entre compilador NSL e Runtime deverá ser versionado.

### 12.4 Compilador Nebula

A comunicação deverá ocorrer por interfaces formais, diagnósticos cruzados, dependências e otimizações coordenadas. Tipos compartilhados poderão ser utilizados quando houver contrato explícito e compatível.

### 12.5 Separação de responsabilidades

A apresentação visual deverá permanecer separada da lógica de aplicação. Contratos, interfaces, tipos e dependências deverão controlar a comunicação. NSL não deverá tornar uma folha de estilos um canal irrestrito para executar lógica arbitrária.

## 13. Segurança, permissões e isolamento

### 13.1 Validação e prevenção

O sistema deverá incluir validação estática, restrições de execução, prevenção de injeções, controle de recursos e diagnósticos. Valores e recursos externos deverão ser tratados conforme seu contexto e origem.

### 13.2 Recursos externos

O carregamento deverá respeitar políticas de origem, CSP, CORS, sandbox, integridade e permissões. Recursos não autorizados deverão ser bloqueados ou substituídos por fallback seguro, com diagnóstico.

### 13.3 APIs do navegador

A integração deverá utilizar APIs tipadas, detecção de compatibilidade e permissões explícitas. NSL não poderá ampliar as capacidades de segurança concedidas pelo navegador.

### 13.4 Recursos gráficos

A utilização de APIs gráficas e aceleração deverá respeitar capacidades, isolamento e políticas do navegador. Recursos e buffers deverão ter ciclo de vida e liberação definidos.

### 13.5 ZenithOS e Portunus

A integração especial com ABI/syscalls do kernel ZenithOS é uma capacidade separada do NSL geral e não será concedida a documentos Web. Somente Portunus executando dentro do ZenithOS poderá utilizar interfaces autorizadas e mediadas pelo kernel. NSL não dará acesso direto a hardware nem a syscalls arbitrárias.

## 14. Compatibilidade e interoperabilidade CSS

NSL deverá oferecer conversão e interoperabilidade bidirecional quando houver equivalência semântica. O sistema deverá incluir mapeamento de propriedades, importação, exportação, fallback e diagnósticos de incompatibilidade.

A conversão não deverá afirmar equivalência quando as semânticas forem diferentes. Nesses casos, deverá:
1. preservar a informação em representação intermediária, se possível;
2. emitir diagnóstico preciso;
3. aplicar fallback documentado, quando seguro;
4. rejeitar conversão que altere comportamento de forma não autorizada.

A matriz de navegadores, versões e funcionalidades deverá ser definida separadamente e atualizada conforme a política de suporte do projeto.

## 15. Renderização, carregamento e ciclo de vida

### 15.1 Renderização

O sistema deverá suportar renderização convencional e incremental, atualização reativa, agendamento, priorização, composição e otimização de atualizações.

### 15.2 SSR e hidratação

A arquitetura deverá suportar SSR, hidratação seletiva, renderização incremental, streaming e compatibilidade. A especificação deverá definir a correspondência entre estilos gerados no servidor e no cliente, transferência de estado e recuperação em caso de divergência.

### 15.3 Estilos críticos

O compilador deverá analisar dependências, extrair estilos críticos, priorizar recursos e realizar divisão automática e carregamento otimizado.

### 15.4 Carregamento

Deverá oferecer carregamento incremental, modular, sob demanda, priorização, dependências, cache e cancelamento. A ordem de carregamento deverá preservar a cascata e a semântica dos módulos.

### 15.5 Memória e recursos

O runtime deverá controlar alocação, reutilização, descarte, cache e monitoramento, com mecanismos de prevenção e diagnóstico de vazamentos. A implementação deverá respeitar o gerenciamento de memória e as capacidades do ambiente de execução.

### 15.6 Cache

Deverá suportar cache incremental, invalidação inteligente, dependências e persistência quando disponível e permitida. A invalidação deverá impedir uso de estilos obsoletos após alterações relevantes.

### 15.7 Processamento paralelo

A arquitetura deverá permitir processamento paralelo seguro de tarefas independentes, análise incremental e compilação concorrente. A paralelização deverá preservar determinismo e não poderá introduzir condições de corrida em estruturas compartilhadas.

## 16. Compilador e representação intermediária

### 16.1 Pipeline

A arquitetura do compilador deverá contemplar, no mínimo:

1. leitura de fonte, codificação e localização;
2. análise léxica e diagnósticos;
3. análise sintática;
4. construção e validação da AST;
5. resolução de módulos, nomes e escopos;
6. análise semântica e sistema de tipos;
7. validação de propriedades, valores e unidades;
8. análise de seletores, cascata e dependências;
9. análise estática e otimizações;
10. geração e validação de representação intermediária (IR);
11. planejamento de renderização e carregamento;
12. geração de CSS e demais artefatos interoperáveis;
13. mapas de origem e informações de depuração;
14. integração com Runtime NML/Nebula;
15. testes de conformidade.

A organização física dos módulos e executáveis será definida pela arquitetura do repositório. A lista define responsabilidades lógicas, não exige um processo separado por etapa.

### 16.2 AST

A AST deverá representar de forma tipada e estável regras, seletores, declarações, expressões, propriedades, módulos, condições, temas e construções da linguagem. Deverá preservar localização de origem e invariantes verificáveis.

### 16.3 IR

A representação intermediária deverá permitir validação, análise, otimização e geração de saídas sem depender diretamente da sintaxe de origem. Seu formato e versionamento deverão ser documentados.

### 16.4 Otimizações

Deverá oferecer análise estática, tree shaking, minificação, deduplicação, divisão de estilos, carregamento sob demanda, cache e otimização contextual. Toda transformação deverá preservar a semântica observável ou ser explicitamente condicionada por uma regra normativa.

### 16.5 Geração de saída

A geração deverá produzir CSS interoperável, representação intermediária quando aplicável, mapas de origem, artefatos de depuração e integração com Runtime NML. Os formatos e contratos deverão ser versionados.

## 17. Módulos, plugins e bibliotecas

### 17.1 Sistema de módulos

Deverá oferecer módulos tipados, namespaces, dependências, interfaces, versionamento, resolução de conflitos e carregamento modular. A resolução deverá ser determinística e compatível com builds reproduzíveis.

### 17.2 Bibliotecas visuais

Deverá suportar bibliotecas modulares com componentes, temas, APIs, versionamento, dependências e distribuição. Interfaces públicas deverão ser documentadas e compatíveis com as políticas de versão.

### 17.3 Plugins

A arquitetura deverá oferecer APIs estáveis para extensões controladas de compilação, propriedades, funções, seletores e ferramentas. Plugins deverão operar em limites definidos, com validação, isolamento e compatibilidade entre versões.

### 17.4 Extensibilidade da linguagem

Configurações, módulos, macros e extensões semânticas controladas poderão ampliar a linguagem. Extensões não poderão alterar silenciosamente a interpretação de construções normativas nem criar ambiguidade na gramática.

### 17.5 Metaprogramação

Deverá suportar macros tipadas, reflexão controlada, geração de código, transformações de AST e validação em compilação. A execução de metaprogramação deverá ocorrer em ambiente controlado e reproduzível.

### 17.6 Distribuição

O ecossistema deverá contemplar pacotes, repositórios, versionamento, dependências, publicação e integração com o gerenciador de pacotes Nebula.

## 18. Ferramentas de desenvolvimento

### 18.1 Diagnósticos

O sistema deverá fornecer erros, avisos, sugestões, rastreamento de origem, análise de conflitos, acessibilidade e desempenho. Diagnósticos deverão possuir identificadores estáveis, severidade, localização, explicação e referência à regra aplicável.

### 18.2 Depurador

Deverá permitir inspeção em tempo real, árvore de estilos, cascata explicada, valores computados, dependências, profiler e integração com debugger Nebula.

### 18.3 Rastreamento

Deverá oferecer mapas de origem, cadeia de dependências, origem de valores e histórico de transformações. Será possível relacionar o estilo computado ao arquivo, módulo, regra e expressão que o originaram.

### 18.4 Formatador

O formatador oficial deverá oferecer configuração, preservação de comentários, organização de módulos, correções seguras e integração com editores.

### 18.5 Lint

O lint deverá oferecer regras extensíveis, análise estática, acessibilidade, segurança, desempenho, correções automáticas e integração contínua.

### 18.6 Documentação automática

Deverá gerar documentação de módulos, propriedades, tipos, componentes, dependências e exemplos, com integração às ferramentas Nebula.

### 18.7 IDE e LSP

O ecossistema deverá oferecer LSP, realce, autocompletar, diagnósticos, navegação, refatoração, sugestões inteligentes, depuração, documentação contextual e análise em tempo real. A extensão oficial Nebula deverá integrar essas capacidades conforme forem implementadas.

### 18.8 Refatoração

Deverá suportar análise de impacto, renomeação, extração, reorganização, migrações e correções seguras, preservando comportamento sempre que a transformação puder ser comprovada.

### 18.9 Monitoramento

Deverá medir renderização, animações, memória, atualizações e gargalos, produzindo relatórios e integração com ferramentas de depuração. Métricas e metodologia deverão ser documentadas.

## 19. Acessibilidade

A acessibilidade será requisito transversal do NSL. A linguagem e suas ferramentas deverão oferecer:

- análise de contraste;
- foco visível e estados de interação;
- legibilidade e tipografia adaptável;
- respeito a movimento reduzido;
- suporte a preferências de contraste e cores;
- indicadores que não dependam exclusivamente de cor;
- integração com semântica e árvore de acessibilidade da plataforma;
- diagnósticos automáticos e recomendações;
- validação de componentes e bibliotecas;
- testes automatizados e regressão visual.

A ferramenta deverá distinguir problemas que podem ser verificados automaticamente daqueles que exigem avaliação humana. Nenhum relatório automatizado deverá ser tratado como garantia absoluta de conformidade de acessibilidade.

## 20. Testes, qualidade e conformidade

### 20.1 Testes automatizados

O programa de testes deverá abranger:
- testes unitários;
- testes de lexer, parser e AST;
- testes de tipos e análise semântica;
- testes de cascata e especificidade;
- testes de propriedades e unidades;
- testes de integração com NML e Nebula;
- testes de renderização;
- testes visuais e de regressão;
- testes de acessibilidade;
- testes de segurança;
- testes de desempenho;
- testes de interoperabilidade;
- testes ponta a ponta;
- testes em navegadores e ambientes suportados.

### 20.2 Validação visual

Deverá oferecer snapshots, comparação visual, detecção de diferenças, múltiplos ambientes, análise de acessibilidade e relatórios automatizados. Resultados deverão registrar ambiente e versão do navegador.

### 20.3 Conformidade normativa

A suíte de conformidade deverá testar aceitação e rejeição de sintaxe, semântica, tipos, cascata, renderização, interoperabilidade, segurança e comportamento por versão. Implementações alternativas deverão poder executar os mesmos testes de referência.

### 20.4 CI

Os testes deverão integrar-se à integração contínua. Recursos não implementados não poderão ser contabilizados como aprovados por ausência de testes; deverão estar explicitamente marcados como pendentes ou não suportados.

## 21. Versionamento, compatibilidade e governança

### 21.1 Versionamento

NSL deverá possuir versionamento semântico, identificação de recursos por versão, análise de compatibilidade, migrações e políticas de descontinuação.

### 21.2 Evolução

Mudanças deverão passar por processo formal de proposta, revisão técnica, decisão documentada, versionamento e comunicação de incompatibilidades. Alterações semânticas deverão atualizar a especificação normativa e os testes.

### 21.3 Implementações alternativas

A arquitetura deverá ser independente de uma implementação específica. A especificação, interfaces, testes de conformidade e contratos de interoperabilidade deverão permitir diferentes backends e implementações compatíveis.

### 21.4 Compatibilidade do Runtime

Contratos formais, versionamento, testes de regressão e migrações deverão preservar o comportamento definido pela especificação. A compatibilidade deverá ser avaliada por versão e funcionalidade.

## 22. Arquitetura de implementação de referência

A arquitetura lógica de referência inclui:

1. **Frontend do compilador:** lexer, parser e AST.
2. **Núcleo semântico:** escopos, tipos, resolução de nomes, propriedades e validação.
3. **Motor de seletores:** correspondência, estados, relações e escopos.
4. **Motor de cascata:** origem, camadas, prioridades, especificidade e resolução.
5. **Sistema de valores:** expressões, funções, unidades e dependências.
6. **Sistema de módulos:** imports, exports, bibliotecas, plugins e pacotes.
7. **Analisador estático:** acessibilidade, segurança, conflitos e desempenho.
8. **IR e otimizador:** transformações verificáveis e independentes da sintaxe.
9. **Backend Web:** geração CSS e demais artefatos interoperáveis.
10. **Integração NML:** componentes, árvore, slots, estados e ciclo de vida.
11. **Integração Nebula:** funções, tipos, módulos, dados e diagnósticos.
12. **Runtime:** atualização reativa, carregamento, cache e recursos.
13. **Ferramentas:** LSP, formatador, lint, depurador e profiler.
14. **Conformidade:** testes de referência e validação entre implementações.

Essa arquitetura é uma decomposição lógica. Limites físicos de crates, módulos, processos e pacotes deverão ser decididos durante o desenho técnico.

## 23. Especificação normativa a produzir

A especificação formal deverá definir, no mínimo:

1. codificação e conjunto de caracteres;
2. tokens, comentários e identificadores;
3. palavras reservadas;
4. gramática e regras de parsing;
5. AST e invariantes;
6. escopos e resolução de nomes;
7. sistema de tipos e inferência;
8. propriedades nativas e personalizadas;
9. valores, expressões, funções e unidades;
10. seletores e algoritmo de correspondência;
11. especificidade e cascata;
12. herança, camadas e resolução de conflitos;
13. temas, tokens e variáveis;
14. módulos, mixins, macros e plugins;
15. layout, posicionamento e responsividade;
16. cores, tipografia, imagens e geometria;
17. animações, transições e eventos visuais;
18. componentes, NML e Web Components;
19. integração com Nebula e Runtime;
20. segurança, permissões e isolamento;
21. renderização, SSR, hidratação e carregamento;
22. IR, otimizações e geração de saída;
23. diagnósticos e recuperação;
24. interoperabilidade e compatibilidade;
25. acessibilidade;
26. versionamento e governança;
27. exemplos normativos e não normativos;
28. suíte e critérios de conformidade.

## 24. Decisões consolidadas da entrevista

As 125 perguntas foram respondidas com a opção 4, exceto as cinco primeiras, cujas respostas foram 4, 3, 4, 4 e 4. A tabela abaixo registra os temas e escolhas. A opção escolhida representa o requisito pretendido, não o estado de implementação.

| Nº | Tema | Opção | Decisão consolidada |
|---:|---|:---:|---|
| 1 | Filosofia | 4 | Linguagem própria, avançada e profundamente integrada ao Nebula. |
| 2 | Sintaxe | 3 | Sintaxe original, mantendo conceitos familiares do CSS. |
| 3 | Palavras-chave | 4 | Português como padrão, nomenclatura técnica consistente e integração com Nebula. |
| 4 | Organização | 4 | Arquivos, estilos internos, locais, compartilhados, módulos e composição. |
| 5 | Aplicação de estilos | 4 | Seletores, relações, estados, propriedades, componentes, escopos e condições. |
| 6 | Especificidade | 4 | Cálculo avançado, prioridades configuráveis, escopos e resolução determinística. |
| 7 | Cascata | 4 | Camadas, prioridades, escopos, origem, condições e resolução formal. |
| 8 | Herança | 4 | Herança configurável, propriedades computadas, propagação e isolamento. |
| 9 | Seletores de componentes | 4 | Componentes, slots, estados, propriedades, composição e escopos. |
| 10 | Estados | 4 | Estados de interação, aplicação, componente, dados e integração reativa. |
| 11 | Propriedades visuais | 4 | Propriedades nativas, personalizadas, tipadas e extensíveis. |
| 12 | Valores | 4 | Valores tipados, expressões, cálculos, referências e reatividade. |
| 13 | Unidades | 4 | Unidades absolutas, relativas, contextuais, físicas e personalizadas. |
| 14 | Cores | 4 | Espaços de cor, gradientes, transparência, misturas e perfis. |
| 15 | Tipografia | 4 | Fontes variáveis, carregamento dinâmico e tipografia responsiva. |
| 16 | Box Model | 4 | Dimensões, margens, preenchimentos, bordas e restrições configuráveis. |
| 17 | Layout | 4 | Fluxo, Flexbox, Grid e layouts adaptativos próprios. |
| 18 | Posicionamento | 4 | Posicionamento completo, camadas, âncoras e regras dinâmicas. |
| 19 | Responsividade | 4 | Adaptação por viewport, contêiner, dispositivo e condições. |
| 20 | Media Queries | 4 | Consultas de mídia, recursos, preferências e condições personalizadas. |
| 21 | Container Queries | 4 | Consultas de tamanho, estado, contexto e atualização reativa. |
| 22 | Animações | 4 | Sequências, composição, controle temporal, eventos e integração Nebula. |
| 23 | Transições | 4 | Curvas personalizadas, interrupções, reversões e controle programático. |
| 24 | Transformações | 4 | Transformações 2D/3D, matrizes, perspectivas e composição. |
| 25 | Efeitos | 4 | Filtros, sombras, máscaras, recortes e composição visual. |
| 26 | Temas | 4 | Temas dinâmicos, hierarquias, variantes, herança e troca reativa. |
| 27 | Variáveis | 4 | Variáveis tipadas, escopos, dependências e reatividade. |
| 28 | Reutilização | 4 | Módulos, composição, herança, mixins e genéricos. |
| 29 | Escopos | 4 | Escopos globais, locais, hierárquicos e encapsulados. |
| 30 | Composição | 4 | Composição declarativa, hierárquica, condicional e parametrizada. |
| 31 | Mixins | 4 | Parâmetros tipados, padrões, genéricos e validação. |
| 32 | Propriedades personalizadas | 4 | Tipos, restrições, herança, dependências e reatividade. |
| 33 | Condicionais | 4 | Condições tipadas, expressões, estados e ambiente. |
| 34 | Geração de regras | 4 | Iteração, filtros e geração estática e dinâmica. |
| 35 | Funções Nebula | 4 | Integração tipada com funções, módulos, eventos e reatividade. |
| 36 | Atualização | 4 | Dependências reativas, atualização incremental e agendamento. |
| 37 | Otimização | 4 | Análise estática, tree shaking, minificação e divisão de estilos. |
| 38 | CSS | 4 | Interoperabilidade bidirecional, mapeamento, fallbacks e diagnósticos. |
| 39 | HTML | 4 | Reconhecimento e estilização de elementos HTML e personalizados. |
| 40 | APIs do navegador | 4 | APIs tipadas, detecção de compatibilidade e permissões. |
| 41 | Diagnósticos | 4 | Erros, avisos, sugestões, conflitos, acessibilidade e desempenho. |
| 42 | Depuração | 4 | Inspeção em tempo real, cascata, valores computados e profiler. |
| 43 | Acessibilidade visual | 4 | Validação, contraste, foco, legibilidade e preferências. |
| 44 | Testes | 4 | Testes unitários, integração, visuais, regressão, desempenho e acessibilidade. |
| 45 | Documentação | 4 | Gramática, semântica, tipos, APIs, exemplos e conformidade. |
| 46 | Unidades personalizadas | 4 | Unidades, dimensões, conversões e validação tipada. |
| 47 | Imagens e recursos | 4 | Imagens, SVG, vetores, máscaras e carregamento otimizado. |
| 48 | Bordas e contornos | 4 | Bordas tipadas, formas, contornos e recortes. |
| 49 | Camadas visuais | 4 | Hierarquia, isolamento, composição e gerenciamento de camadas. |
| 50 | Geometria | 4 | Geometria 2D/3D, caminhos, curvas e operações geométricas. |
| 51 | Filtros | 4 | Pipelines tipados, composição e aceleração quando disponível. |
| 52 | Gráficos | 4 | Escalas, eixos, legendas, temas, animações e dados Nebula. |
| 53 | Impressão | 4 | Paginação, margens, cabeçalhos, rodapés e perfis. |
| 54 | Dispositivos de entrada | 4 | Mouse, toque, caneta, teclado e capacidades. |
| 55 | Preferências | 4 | Temas, movimento, contraste, transparência e densidade. |
| 56 | Internacionalização visual | 4 | Direção, idiomas, tipografia e layouts culturais. |
| 57 | Densidade visual | 4 | Densidade global, contextual, adaptativa e configurável. |
| 58 | Extensões | 4 | Plugins e APIs públicas para recursos extensíveis. |
| 59 | Bibliotecas | 4 | Bibliotecas modulares, componentes, temas e distribuição. |
| 60 | Compatibilidade entre versões | 4 | Versionamento semântico, migrações e análise de compatibilidade. |
| 61 | Carregamento | 4 | Carregamento incremental, modular, priorizado e sob demanda. |
| 62 | Cache | 4 | Cache incremental, invalidação inteligente e dependências. |
| 63 | Paralelismo | 4 | Processamento paralelo seguro e compilação concorrente. |
| 64 | Compilador | 4 | Lexer, Parser, AST, semântica, tipos, IR e geração. |
| 65 | Saída | 4 | CSS, IR, mapas de origem e artefatos de depuração. |
| 66 | Sistema de tipos | 4 | Tipos estáticos, inferência, genéricos e restrições. |
| 67 | Definição de propriedades | 4 | Declaração, registro, tipagem, validação e dependências. |
| 68 | Funções nativas | 4 | Biblioteca matemática, geométrica, tipográfica e reativa. |
| 69 | Expressões | 4 | Avaliação tipada, dependências e análise estática/dinâmica. |
| 70 | Conflitos | 4 | Resolução determinística e explicável. |
| 71 | Isolamento de componentes | 4 | Encapsulamento e controle de fronteiras. |
| 72 | Proteção | 4 | Validação, restrições, prevenção de injeções e controle de recursos. |
| 73 | Recursos externos | 4 | Permissões, políticas de origem e integridade. |
| 74 | Segurança do navegador | 4 | Integração com sandbox, CSP, CORS e permissões. |
| 75 | Separação de lógica | 4 | Contratos e interfaces entre apresentação e aplicação. |
| 76 | Integração NML | 4 | Integração profunda com árvore, componentes, estados e eventos. |
| 77 | Runtime Web | 4 | Integração com ciclo de vida, renderização e reatividade. |
| 78 | Compilador Nebula | 4 | Interfaces formais e diagnósticos e otimizações coordenadas. |
| 79 | Componentes reutilizáveis | 4 | Contratos visuais, variantes, slots e bibliotecas. |
| 80 | Dados dinâmicos | 4 | Vinculação tipada, dependências e atualização incremental. |
| 81 | Especificação formal | 4 | Especificação normativa de gramática, semântica e comportamento. |
| 82 | Conformidade | 4 | Testes normativos e interoperabilidade entre implementações. |
| 83 | Evolução | 4 | Propostas, revisão, versões e migrações. |
| 84 | Distribuição | 4 | Pacotes, repositórios, dependências e publicação. |
| 85 | Implementações futuras | 4 | Arquitetura independente de implementação e backends. |
| 86 | Renderização | 4 | Renderização incremental, paralela quando possível e priorizada. |
| 87 | SSR | 4 | SSR, hidratação seletiva, streaming e integração Runtime. |
| 88 | Estilos críticos | 4 | Extração contextual e carregamento otimizado. |
| 89 | Memória | 4 | Alocação, reutilização, descarte e monitoramento. |
| 90 | Otimização em execução | 4 | Análise e adaptação de desempenho em runtime. |
| 91 | Estados personalizados | 4 | Estados tipados, hierárquicos e reativos. |
| 92 | Eventos visuais | 4 | Eventos tipados, estados e transições. |
| 93 | Gestos | 4 | Reconhecimento, composição e acessibilidade. |
| 94 | Rolagem | 4 | Áreas, encaixe, âncoras e comportamento adaptativo. |
| 95 | Cursores | 4 | Cursores e indicadores contextuais e acessíveis. |
| 96 | Canvas e SVG | 4 | Integração com geometria, efeitos, animação e APIs gráficas. |
| 97 | 3D | 4 | Perspectiva, transformações e integração gráfica. |
| 98 | Composição visual | 4 | Pipelines, camadas, máscaras e mistura. |
| 99 | Filtros personalizados | 4 | Filtros tipados, parâmetros e composição. |
| 100 | Hardware gráfico | 4 | Detecção, backend, fallback e gerenciamento seguro. |
| 101 | Módulos | 4 | Módulos tipados, namespaces, dependências e versionamento. |
| 102 | Metaprogramação | 4 | Macros, reflexão controlada e transformações de AST. |
| 103 | Análise estática | 4 | Tipos, dados, conflitos, acessibilidade, segurança e desempenho. |
| 104 | Plugins de compilação | 4 | APIs estáveis e extensões controladas e isoladas. |
| 105 | Ferramentas externas | 4 | APIs, editores, depuradores, formatadores e CI. |
| 106 | Web Components | 4 | Custom Elements, Shadow DOM, slots e partes exportadas. |
| 107 | Shadow DOM | 4 | Escopos internos, encapsulamento e composição. |
| 108 | Fontes externas | 4 | Carregamento, fallback, fontes variáveis, cache e segurança. |
| 109 | APIs de acessibilidade | 4 | Integração com árvore de acessibilidade e validação. |
| 110 | Navegadores | 4 | Perfis, detecção, fallback e diagnósticos. |
| 111 | Formatador | 4 | Formatação configurável, preservação e correções seguras. |
| 112 | Lint | 4 | Regras extensíveis e análise de segurança, acessibilidade e desempenho. |
| 113 | Documentação automática | 4 | Documentação derivada de módulos e contratos. |
| 114 | IDE | 4 | LSP, sugestões, refatoração, depuração e análise. |
| 115 | Refatoração | 4 | Análise de impacto e transformações seguras. |
| 116 | Erros | 4 | Diagnósticos estruturados, rastreamento e recuperação. |
| 117 | Validação visual | 4 | Snapshots, regressão, múltiplos ambientes e acessibilidade. |
| 118 | Estabilidade do Runtime | 4 | Contratos, versionamento e testes de regressão. |
| 119 | Rastreabilidade | 4 | Mapas de origem, dependências e histórico de transformações. |
| 120 | Monitoramento | 4 | Métricas de renderização, animação, memória e gargalos. |
| 121 | Aplicações grandes | 4 | Arquitetura escalável, incremental e paralela. |
| 122 | Reutilização entre projetos | 4 | Pacotes, interfaces, versões e dependências. |
| 123 | Sistemas de design | 4 | Tokens tipados, temas, componentes e distribuição. |
| 124 | Personalização da linguagem | 4 | Configurações, macros e extensões semânticas controladas. |
| 125 | Visão de longo prazo | 4 | Linguagem independente de apresentação visual, com ecossistema próprio e integração Nebula/NML. |

## 25. Decisões técnicas ainda necessárias

A entrevista definiu o escopo pretendido, mas não escolheu todos os detalhes de engenharia. Antes ou durante a implementação, deverão ser especificados e aprovados:

- gramática concreta e exemplos normativos;
- algoritmo exato de especificidade e cascata;
- catálogo e assinaturas de propriedades e funções;
- sistema de tipos e regras de inferência;
- formato da AST e da IR;
- regras exatas de escopo, herança e encapsulamento;
- contratos de integração NSL–NML–Nebula;
- semântica do runtime reativo;
- arquitetura de SSR e hidratação;
- matriz de navegadores e recursos;
- formatos de pacote, manifests e mapas de origem;
- APIs e modelo de segurança de plugins;
- política de versionamento e governança;
- metas mensuráveis de desempenho e acessibilidade.

Essas definições deverão ser registradas em documentos derivados e decisões arquiteturais. Não se deve inventar sintaxe ou comportamento normativo apenas a partir dos nomes dos recursos.

## 26. Critérios para considerar o NSL concluído

O NSL só poderá ser declarado implementado quando, no mínimo:

1. a especificação normativa estiver completa e sem ambiguidades relevantes;
2. lexer e parser implementarem a gramática;
3. AST e invariantes estiverem definidos e testados;
4. análise semântica e sistema de tipos estiverem implementados;
5. seletores, cascata, herança e escopos tiverem comportamento determinístico;
6. propriedades, valores, unidades e funções estiverem validados;
7. compilador, IR, otimizações e backend gerarem saídas corretas;
8. integração com NML e Nebula estiver coberta por contratos e testes;
9. runtime, renderização, reatividade e carregamento estiverem testados;
10. segurança, acessibilidade e compatibilidade possuírem testes verificáveis;
11. ferramentas de desenvolvimento essenciais estiverem funcionais;
12. testes de conformidade e CI cobrirem os requisitos suportados;
13. limitações e recursos pendentes estiverem documentados;
14. builds e artefatos forem reproduzíveis conforme as garantias definidas.

## 27. Estado do documento

- **Entrevista:** concluída.
- **Perguntas respondidas:** 125.
- **Decisões:** perguntas 1–5 = 4, 3, 4, 4, 4; perguntas 6–125 = opção 4.
- **Requisitos e arquitetura de alto nível:** consolidados neste documento.
- **Gramática normativa:** pendente de elaboração.
- **Implementação do compilador e Runtime:** não declarada concluída.
- **Testes de conformidade:** pendentes de especificação e implementação.

Este documento é a referência consolidada dos requisitos do NSL no Nebula-Web. Mudanças futuras de escopo ou semântica deverão atualizar a versão do documento, preservar o histórico de decisões e manter a distinção entre requisitos planejados e funcionalidades implementadas.
