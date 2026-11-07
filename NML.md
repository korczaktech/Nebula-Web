# NML — Nebula Markup Language
## Especificação de requisitos e arquitetura da linguagem

**Documento:** `NML.md`  
**Projeto:** Nebula-Web  
**Estado:** Especificação de requisitos consolidada — Fase 1  
**Versão do documento:** 0.1.0  
**Idioma da documentação:** Português brasileiro  
**Linguagem de implementação pretendida:** Nebula

---

## 1. Objetivo e escopo

A **Nebula Markup Language (NML)** é a linguagem de marcação do ecossistema Nebula-Web. Seu propósito é descrever documentos, páginas, interfaces, componentes, conteúdo, navegação e comportamento estrutural de aplicações Web por meio de uma linguagem própria, integrada ao ecossistema Nebula.

NML não será apenas uma tradução de HTML. Ela terá identidade sintática e semântica própria, mantendo interoperabilidade com a plataforma Web existente. O compilador deverá transformar documentos NML em artefatos que possam ser executados nos navegadores compatíveis, utilizando HTML, APIs padronizadas e outros mecanismos da plataforma como destino de interoperabilidade.

A linguagem deverá combinar:

- estrutura declarativa de documentos;
- elementos nativos e componentes personalizados;
- composição de páginas e layouts;
- conteúdo dinâmico e integração com código Nebula;
- eventos, navegação e gerenciamento de estado;
- renderização reativa e incremental;
- integração controlada com recursos do navegador;
- compilação, otimização, depuração e distribuição.

Este documento registra as decisões de requisitos coletadas na entrevista da Fase 1. Ele define **o que NML deverá oferecer**, não declara que esses recursos já estejam implementados.

## 2. Princípios fundamentais

### 2.1 Identidade própria

NML deverá possuir sintaxe, gramática, regras semânticas, modelo de componentes e ferramentas próprios. Não deverá ser definida como HTML com palavras-chave traduzidas.

### 2.2 Interoperabilidade

A saída deverá interoperar com HTML, CSS, DOM e APIs padronizadas dos navegadores. Essa interoperabilidade é uma estratégia de execução e compatibilidade, não uma obrigação de expor a sintaxe HTML ao autor de NML.

### 2.3 Integração com Nebula

NML deverá integrar-se ao sistema de módulos, funções, tipos, expressões e mecanismos de execução da linguagem Nebula. A integração deverá ocorrer por interfaces e contratos definidos, sem misturar indevidamente a gramática ou a semântica das duas linguagens.

### 2.4 Separação entre NML e NSL

NML será responsável pela estrutura, conteúdo e composição da interface. NSL será responsável pela apresentação visual e pelos estilos. As linguagens terão especificações e implementações independentes, mas mecanismos explícitos de integração.

NML deverá aceitar:
- arquivos NSL externos, com extensão `.nsl`;
- estilos definidos no próprio documento NML;
- estilos locais a componentes;
- estilos compartilháveis entre componentes e documentos.

A semântica detalhada dos estilos pertence à especificação NSL.

### 2.5 Segurança por padrão

Conteúdo NML não poderá obter acesso arbitrário ao sistema operacional, ao hardware ou a chamadas de sistema. O acesso a recursos do navegador deverá respeitar permissões, políticas de origem, sandbox e mecanismos de segurança da plataforma.

A integração especial com a ABI/syscalls do ZenithOS será restrita ao navegador Portunus executado dentro do ZenithOS, mediada e autorizada pelo kernel. Essa integração não será uma capacidade geral de documentos Web nem permitirá que páginas executem syscalls arbitrárias.

### 2.6 Implementação e testes

A implementação própria deverá ser escrita em Nebula, conforme a direção arquitetural do Nebula-Web. Cada subsistema deverá possuir especificação, implementação e testes antes de ser considerado concluído.

## 3. Filosofia e características da linguagem

### 3.1 Modelo híbrido

NML será uma linguagem híbrida: oferecerá recursos familiares de linguagens de marcação e funcionalidades próprias que ultrapassam a descrição estática de documentos.

O modelo híbrido deverá preservar uma separação compreensível entre:
- estrutura e conteúdo;
- apresentação visual;
- dados e estado;
- lógica de aplicação;
- integração com APIs externas.

### 3.2 Sintaxe original

A sintaxe de NML será original. Não há requisito de reproduzir tags, atributos, delimitadores ou convenções do HTML. A forma concreta da sintaxe deverá ser definida em uma etapa própria de especificação gramatical.

Nenhum exemplo ilustrativo neste documento deverá ser interpretado como sintaxe normativa, a menos que seja explicitamente marcado como tal em uma futura versão da gramática.

### 3.3 Idioma das palavras-chave

As palavras-chave e construções reservadas de NML deverão seguir a identidade em português do ecossistema Nebula, preferencialmente em português brasileiro e sem acentos, mantendo consistência com as convenções de Nebula.

Nomes definidos pelo autor, identificadores de componentes, módulos e propriedades deverão seguir regras próprias de identificadores que serão formalizadas na gramática.

### 3.4 Estrutura documental híbrida

A organização dos documentos combinará regras obrigatórias com liberdade estrutural. A especificação formal deverá identificar claramente:
- estruturas obrigatórias;
- estruturas opcionais;
- regiões de conteúdo livre;
- regras de aninhamento;
- limites de escopo;
- regras de composição;
- condições de validade de um documento.

A liberdade estrutural não poderá tornar ambíguos o parsing, a análise semântica ou a renderização.

### 3.5 Hierarquia

A hierarquia dos documentos será híbrida. Determinados níveis e relações estruturais serão obrigatórios; outros poderão ser organizados livremente pelo autor, desde que respeitem as regras de validade e semântica.

### 3.6 Tipagem

NML adotará um modelo híbrido:
- elementos nativos terão contratos, propriedades e restrições tipadas rigorosas;
- componentes personalizados poderão definir contratos e graus de flexibilidade próprios;
- dados, propriedades, atributos e vinculações deverão ser validados de acordo com o sistema de tipos aplicável;
- a flexibilidade de componentes personalizados não poderá eliminar verificações de segurança ou coerência estrutural.

## 4. Elementos, componentes e composição

### 4.1 Biblioteca de elementos nativos

NML deverá fornecer uma biblioteca completa de elementos nativos, organizada por categorias. A biblioteca deverá documentar, para cada elemento:
- finalidade e semântica;
- categoria;
- propriedades e atributos aceitos;
- tipos e valores permitidos;
- comportamento padrão;
- eventos disponíveis;
- regras de conteúdo e aninhamento;
- requisitos de acessibilidade;
- comportamento de renderização;
- compatibilidade com navegadores;
- restrições de segurança;
- alternativas ou fallbacks quando aplicáveis.

A biblioteca deverá ser extensível por mecanismos formais, sem permitir que extensões alterem silenciosamente a semântica dos elementos fundamentais.

### 4.2 Componentes personalizados

Autores deverão poder definir componentes reutilizáveis e integrá-los diretamente a funções e módulos Nebula. Componentes poderão encapsular estrutura, propriedades, estado, eventos, estilos e comportamento.

O sistema deverá definir:
- declaração e exportação de componentes;
- importação e reutilização;
- propriedades de entrada e seus tipos;
- valores padrão e validação;
- composição de componentes;
- comunicação entre componentes;
- slots e regiões substituíveis;
- estado interno e exposição controlada de estado;
- eventos emitidos e recebidos;
- ciclo de vida;
- execução síncrona e assíncrona;
- regras de isolamento e segurança.

### 4.3 Sistema avançado de componentes visuais

O modelo de componentes deverá suportar composição, slots, propriedades, estados, ciclos de vida, componentes assíncronos e componentes dinâmicos.

Componentes dinâmicos poderão ser selecionados ou construídos durante a execução dentro dos limites definidos pelo sistema de tipos, pelas permissões e pelo modelo de renderização.

### 4.4 Modelos e layouts

NML deverá possuir um sistema de modelos reutilizáveis de páginas. Esse sistema deverá suportar:
- composição e, quando formalmente apropriado, herança de modelos;
- parâmetros tipados;
- regiões substituíveis;
- layouts aninhados;
- páginas derivadas de layouts compartilhados;
- integração com componentes e módulos Nebula;
- validação de compatibilidade entre o modelo e os dados fornecidos.

A especificação deverá definir claramente a precedência e a resolução de conflitos entre composição, herança e substituição de regiões.

### 4.5 Organização de páginas e documentos

Um projeto NML deverá poder organizar páginas, layouts, rotas, subpáginas e documentos compartilhados. A composição deverá permitir carregamento dinâmico e reutilização sem exigir que todo o conteúdo esteja concentrado em um único arquivo.

O sistema de projetos deverá integrar-se ao gerenciamento de módulos, dependências e configurações por ambiente do ecossistema Nebula-Web.

## 5. Conteúdo dinâmico, controle e dados

### 5.1 Expressões Nebula

NML deverá permitir integração completa com Nebula nos contextos expressamente autorizados pela gramática. Expressões válidas poderão alimentar conteúdo, propriedades, atributos, condições, parâmetros, eventos e outras operações previstas.

A integração não significa que qualquer trecho arbitrário de código Nebula poderá ser inserido em qualquer posição. Cada contexto deverá definir:
- quais expressões são permitidas;
- quais tipos são aceitos;
- se a avaliação é síncrona ou assíncrona;
- quais efeitos são permitidos;
- quais permissões são necessárias;
- como erros são apresentados e recuperados.

### 5.2 Condicionais, repetição e filtros

A linguagem deverá oferecer controle estrutural completo, incluindo:
- renderização condicional;
- alternativas condicionais;
- repetição e iteração;
- filtros sobre coleções;
- iteração assíncrona;
- tratamento de coleções vazias;
- identificação estável de itens quando necessária;
- otimizações de atualização e renderização.

As regras deverão evitar atualizações incorretas quando coleções mudarem, itens forem reordenados ou operações assíncronas terminarem fora de ordem.

### 5.3 Vinculação de dados

O sistema deverá suportar vinculação:
- unidirecional;
- bidirecional;
- reativa;
- derivada;
- assíncrona.

As vinculações deverão possuir validação de tipos e regras explícitas para propagação de alterações, resolução de conflitos, valores ausentes, falhas e cancelamento.

A vinculação bidirecional deverá ser usada somente em contextos nos quais a semântica de leitura e escrita seja definida. O compilador deverá detectar incompatibilidades quando possível.

### 5.4 Estado da aplicação

NML deverá integrar-se a um sistema de estado que contemple:
- estados locais de componentes;
- estados globais compartilhados;
- estados derivados;
- estados persistentes;
- estados reativos;
- estados assíncronos.

A especificação deverá definir escopo, propriedade, ciclo de vida, atualização, observação, persistência, serialização, sincronização e liberação de recursos de cada tipo de estado.

Estados globais deverão possuir mecanismos de isolamento e controle de acesso para evitar dependências acidentais entre partes não relacionadas da aplicação.

## 6. Eventos, navegação e formulários

### 6.1 Sistema de eventos

NML deverá possuir um sistema próprio de eventos que inclua:
- eventos predefinidos;
- eventos personalizados;
- integração com funções Nebula;
- propagação e tratamento;
- cancelamento quando permitido;
- dados tipados associados ao evento;
- regras de ordem de execução;
- tratamento de erros;
- integração com o ciclo de vida.

A especificação deverá definir captura, propagação, escopo, prioridade e prevenção de comportamentos padrão, respeitando os mecanismos e limites do navegador.

### 6.2 Navegação e rotas

O sistema de navegação deverá oferecer:
- declaração e organização de rotas;
- parâmetros de rota;
- rotas aninhadas;
- navegação programática;
- histórico de navegação;
- integração com funções e módulos Nebula;
- carregamento associado a rotas;
- tratamento de rotas inexistentes;
- fallbacks e redirecionamentos;
- políticas de acesso e autorização no contexto da aplicação.

A navegação NML deverá interoperar com o mecanismo de URL e histórico do navegador, sem presumir que a aplicação controla a infraestrutura do servidor.

### 6.3 Formulários

O sistema de formulários deverá incluir:
- campos nativos e personalizados;
- contratos tipados para valores;
- validação local e integrada a módulos Nebula;
- validação síncrona e assíncrona;
- estados de formulário e de campo;
- eventos;
- mensagens de erro acessíveis;
- envio assíncrono;
- controle de envio repetido;
- cancelamento quando aplicável;
- integração com serviços e APIs;
- suporte a formulários compostos.

A validação no cliente melhora a experiência, mas não substitui validação e autorização no servidor.

## 7. Multimídia e acessibilidade

### 7.1 Multimídia

A linguagem deverá oferecer suporte completo a imagens, vídeos e áudio, incluindo:
- declaração e apresentação de recursos;
- carregamento dinâmico;
- carregamento sob demanda;
- streaming quando suportado pela plataforma;
- controles e eventos;
- processamento por APIs permitidas;
- integração com módulos Nebula;
- formatos alternativos e fallbacks;
- gerenciamento de falhas e cancelamento.

O processamento deverá respeitar permissões, capacidade do dispositivo, políticas de reprodução e limitações do navegador.

### 7.2 Acessibilidade

A acessibilidade será requisito fundamental e deverá contemplar:
- padrões e recomendações internacionais aplicáveis;
- semântica acessível;
- navegação por teclado;
- foco e ordem de foco;
- nomes, papéis e estados acessíveis;
- suporte a tecnologias assistivas;
- alternativas textuais e legendas;
- validação automática;
- diagnóstico de problemas;
- requisitos obrigatórios para elementos e componentes nativos;
- orientação para componentes personalizados.

A ferramenta de desenvolvimento deverá identificar violações detectáveis automaticamente. A aprovação em testes automatizados não deverá ser apresentada como garantia absoluta de acessibilidade.

## 8. Comentários, atributos, metadados e módulos

### 8.1 Comentários

A linguagem deverá suportar:
- comentários de uma linha;
- comentários de múltiplas linhas;
- documentação de componentes;
- comentários condicionais, conforme regras formais;
- metadados destinados a ferramentas.

Comentários condicionais e metadados deverão ter semântica definida e não poderão criar diferenças imprevisíveis entre ferramentas.

### 8.2 Atributos

O modelo de atributos deverá suportar:
- atributos tipados;
- atributos personalizados;
- valores dinâmicos;
- herança quando definida;
- reatividade;
- integração com funções e módulos Nebula.

A especificação deverá diferenciar atributos estruturais, propriedades de componente, dados, metadados e propriedades refletidas no elemento de destino. Regras de herança e precedência deverão ser explícitas.

### 8.3 Metadados

NML deverá oferecer metadados tradicionais e personalizados, incluindo categorias para:
- identificação e descrição do documento;
- SEO;
- acessibilidade;
- segurança;
- configuração de execução;
- integração com módulos Nebula;
- ferramentas de análise e distribuição.

Metadados desconhecidos deverão seguir regras de preservação, validação ou rejeição definidas pela especificação.

### 8.4 Importação, exportação e dependências

O sistema de módulos deverá suportar:
- importações e exportações;
- namespaces;
- dependências;
- versões;
- resolução de módulos;
- integração com o sistema de módulos Nebula;
- verificação de dependências;
- detecção de ciclos quando aplicável;
- controle de visibilidade e encapsulamento.

A resolução deverá ser determinística e documentada para builds reproduzíveis.

## 9. Segurança e permissões

### 9.1 Modelo de segurança

NML deverá adotar um modelo abrangente de segurança com:
- isolamento de componentes e contextos quando necessário;
- permissões granulares;
- proteção contra injeções;
- validação e tratamento seguro de dados;
- controle de acesso a recursos;
- políticas de conteúdo;
- integração com mecanismos de proteção do navegador.

A segurança não deverá depender apenas de convenções do autor. O compilador e o runtime deverão aplicar as restrições que forem de sua responsabilidade.

### 9.2 Acesso a APIs

O acesso a APIs do navegador deverá utilizar interfaces tipadas e mecanismos explícitos de permissão. O sistema deverá oferecer detecção de compatibilidade e integração com módulos Nebula, sem contornar políticas de segurança da plataforma.

### 9.3 Recursos externos

Para recursos provenientes de outros domínios ou origens, NML deverá integrar:
- políticas de origem;
- verificação de integridade quando aplicável;
- permissões;
- políticas de conteúdo;
- proteção contra recursos maliciosos;
- mecanismos de segurança fornecidos pelo navegador.

A linguagem deverá respeitar CORS, CSP, sandbox, política de mesma origem e demais controles da plataforma quando aplicáveis. Não deverá prometer acesso que o navegador ou o servidor não autorizem.

### 9.4 Limite de integração com ZenithOS

Somente o Portunus em execução dentro do ZenithOS poderá utilizar a integração especial prevista com a ABI/syscalls oficiais do kernel. O kernel deverá mediar e autorizar cada capacidade. Documentos NML, scripts e componentes Web não receberão acesso direto a hardware nem a syscalls arbitrárias.

## 10. Renderização e ciclo de vida

### 10.1 Modelo de renderização

NML deverá suportar um modelo híbrido de renderização, contemplando:
- renderização no servidor (SSR);
- renderização no cliente (CSR);
- hidratação;
- atualização reativa;
- renderização incremental.

A especificação deverá definir os cenários de uso, requisitos de compatibilidade, transferência de estado e comportamento quando a hidratação falhar ou o HTML recebido não corresponder à estrutura esperada.

### 10.2 Controle de renderização

O sistema deverá combinar atualização automática e controle manual. Deverá incluir:
- atualização reativa;
- renderização incremental;
- agendamento;
- priorização;
- mecanismos para evitar trabalho redundante;
- controle manual pelo desenvolvedor;
- estratégias de atualização adequadas a componentes e páginas.

O agendamento deverá respeitar as prioridades da plataforma e evitar bloquear desnecessariamente a interface.

### 10.3 Ciclo de vida das páginas

O ciclo de vida deverá contemplar etapas de:
- inicialização;
- criação;
- montagem;
- atualização;
- suspensão;
- retomada;
- destruição;
- recuperação.

Cada etapa deverá ter eventos e contratos documentados, incluindo ordem, condições de execução, erros possíveis e recursos que devem ser liberados.

### 10.4 Ciclo de vida dos componentes

Componentes deverão possuir ciclos de vida síncronos e assíncronos, com:
- inicialização;
- criação e montagem;
- atualização;
- dependências;
- efeitos;
- limpeza;
- suspensão;
- retomada;
- destruição;
- controle de execução.

A execução assíncrona deverá definir cancelamento, concorrência, resultados obsoletos e liberação de recursos.

### 10.5 Recuperação de erros de execução

O runtime deverá suportar:
- limites de erro;
- isolamento de componentes com falha;
- recuperação automática configurável;
- fallbacks visuais;
- registro estruturado de erros;
- integração com diagnósticos Nebula.

Uma falha isolada deverá poder ser contida sem necessariamente interromper a página inteira. A política de recuperação deverá evitar ocultar erros graves ou deixar a interface em estado inconsistente.

## 11. Rede, processamento paralelo e funcionamento offline

### 11.1 Comunicação com servidores

O sistema deverá oferecer uma camada de comunicação que contemple:
- HTTP;
- APIs REST;
- WebSocket;
- Server-Sent Events;
- streaming;
- cancelamento;
- interceptadores;
- integração com módulos Nebula.

A camada deverá definir tipagem de requisições e respostas, tratamento de erros, timeouts, cancelamento, autenticação por mecanismos permitidos, políticas de origem e observabilidade. Não deverá presumir que o cliente controla ou pode confiar no servidor remoto.

### 11.2 Processamento paralelo

NML deverá integrar recursos de processamento em segundo plano, incluindo:
- Web Workers;
- Shared Workers quando disponíveis;
- comunicação entre tarefas;
- filas;
- cancelamento;
- transferência de dados;
- integração com Nebula.

A implementação deverá detectar suporte da plataforma e fornecer fallbacks quando possível. Não deverá pressupor que toda API esteja disponível em todos os navegadores.

### 11.3 Recursos em segundo plano

O sistema deverá integrar:
- notificações, mediante permissão;
- sincronização em segundo plano, quando suportada;
- tarefas agendadas dentro das limitações da plataforma;
- gerenciamento de recursos;
- Service Workers.

A especificação deverá distinguir tarefas que podem continuar após a página ser fechada daquelas que dependem de uma página ativa. O navegador e o sistema operacional mantêm o controle final sobre a execução em segundo plano.

### 11.4 Aplicações offline e PWA

NML deverá permitir aplicações com funcionamento offline, incluindo:
- estratégias de cache;
- cache inteligente;
- armazenamento local;
- detecção de conectividade;
- sincronização posterior;
- atualização de recursos;
- integração com Progressive Web Apps (PWA).

A política de cache deverá definir validade, atualização, invalidação, versionamento e tratamento de dados desatualizados. A sincronização deverá tratar conflitos e falhas de conectividade.

## 12. Armazenamento e gerenciamento de recursos

### 12.1 Armazenamento no navegador

NML deverá oferecer uma abstração própria para armazenamento no navegador, integrando:
- armazenamento local;
- armazenamento de sessão;
- IndexedDB;
- cookies;
- cache;
- controle de permissões.

A abstração deverá definir seleção do mecanismo, limites, serialização, expiração, migrações, tratamento de erros, privacidade e limpeza. Cookies deverão respeitar atributos e restrições de segurança definidos pela plataforma. O acesso a armazenamento não poderá ignorar permissões ou políticas de privacidade.

### 12.2 Catálogo de recursos

O gerenciamento de recursos deverá possuir catálogo e metadados para imagens, fontes, vídeos, áudios e outros arquivos. Deverá oferecer:
- otimização;
- conversão;
- compressão;
- carregamento sob demanda;
- cache;
- integridade;
- versionamento.

As transformações deverão preservar ou declarar explicitamente alterações de formato, qualidade e compatibilidade.

### 12.3 Carregamento de módulos e recursos

O carregamento deverá ser assíncrono e permitir:
- priorização;
- resolução de dependências;
- divisão de recursos;
- cache;
- pré-carregamento;
- cancelamento;
- carregamento sob demanda.

O compilador deverá analisar dependências para reduzir carregamentos desnecessários, respeitando efeitos colaterais e semântica dos módulos.

## 13. SEO, compatibilidade e internacionalização

### 13.1 SEO e indexação

NML deverá oferecer recursos completos de SEO e indexação:
- metadados para mecanismos de busca;
- dados estruturados;
- geração ou integração com sitemap;
- controle de rastreamento;
- renderização otimizada para buscadores;
- ferramentas de análise.

O comportamento deverá considerar SSR e os mecanismos efetivamente suportados pelos buscadores. A linguagem não poderá garantir indexação ou posicionamento, pois essas decisões pertencem aos mecanismos externos.

### 13.2 Compatibilidade com navegadores

O ecossistema deverá possuir uma camada própria de compatibilidade com:
- detecção de recursos;
- identificação de capacidades;
- fallbacks;
- polyfills quando apropriados;
- saída compatível com os principais navegadores suportados.

A matriz de navegadores e versões mínimas deverá ser definida em documento próprio e atualizada conforme a política de suporte do projeto. Polyfills deverão ser selecionados de forma explícita e segura, evitando inclusão desnecessária.

### 13.3 Internacionalização

A integração com o sistema de internacionalização do ecossistema deverá contemplar:
- traduções;
- localização;
- formatação regional;
- pluralização;
- interpolação;
- fallback de idiomas;
- carregamento dinâmico de traduções;
- detecção regional;
- integração com Nebula.

A especificação deverá definir resolução de idioma, precedência entre preferências, comportamento para traduções ausentes, formatos de data, hora, número e moeda, além do tratamento de conteúdo com direção de escrita distinta.

## 14. Desempenho e otimização

NML deverá possuir análise estática e otimizações para desenvolvimento e produção, incluindo:
- tree shaking;
- divisão de código;
- carregamento sob demanda;
- cache;
- otimização de renderização;
- builds incrementais;
- análise de dependências;
- redução de recursos não utilizados;
- otimização configurável por ambiente.

O compilador deverá preservar a semântica do programa. Otimizações que possam alterar ordem de efeitos, eventos ou comportamento observável deverão ser proibidas ou condicionadas a provas e regras explícitas.

A ferramenta deverá oferecer diagnósticos que expliquem oportunidades de otimização sem transformar heurísticas em garantias de desempenho.

## 15. Ferramentas de desenvolvimento

O ecossistema deverá oferecer ambiente completo de desenvolvimento, com:
- depuração;
- inspeção visual;
- source maps;
- profiler;
- monitoramento reativo;
- análise de rede;
- integração com o debugger Nebula;
- inspeção de componentes e estados;
- visualização de erros e diagnósticos;
- análise de acessibilidade;
- análise de desempenho.

As ferramentas deverão permitir relacionar o artefato executado ao documento NML original. Source maps e informações de depuração deverão ser produzidos de forma consistente nos modos em que forem habilitados.

## 16. Estrutura de projetos e dependências

O sistema de projetos deverá definir uma estrutura padronizada pelo Nebula-Web, com suporte a:
- múltiplos módulos;
- múltiplos projetos;
- subprojetos;
- configurações por ambiente;
- gerenciamento de dependências;
- recursos compartilhados;
- layouts e páginas;
- ferramentas e testes.

A especificação da estrutura deverá identificar arquivos obrigatórios, opcionais, convenções de nomes, resolução de caminhos, configuração, dependências e limites entre módulos. A estrutura padronizada deverá ser extensível sem comprometer builds reproduzíveis.

## 17. Testes automatizados

O ecossistema NML deverá possuir um sistema completo de testes, contemplando:
- testes unitários;
- testes de integração;
- testes de componentes;
- testes de renderização;
- testes de acessibilidade;
- testes de segurança;
- testes de desempenho;
- testes ponta a ponta (E2E).

A infraestrutura deverá permitir testes determinísticos, isolamento de ambiente, simulação controlada de APIs, verificação de resultados, relatórios e integração com CI.

Testes em navegadores reais deverão complementar testes isolados e de integração. Os resultados deverão identificar ambiente, versão do navegador, capacidades disponíveis e limitações reproduzíveis.

## 18. Compilação, saídas e distribuição

### 18.1 Formatos de saída

O compilador deverá produzir os artefatos necessários à execução e distribuição, incluindo:
- HTML interoperável;
- recursos otimizados;
- metadados;
- manifests;
- arquivos de execução;
- artefatos específicos para diferentes ambientes.

A saída exata dependerá do modo de compilação e do destino selecionado. A especificação deverá separar artefatos obrigatórios de opcionais.

### 18.2 Modos de compilação

Deverão existir modos para:
- desenvolvimento;
- produção;
- depuração;
- testes;
- análise;
- builds incrementais;
- otimização configurável;
- diferentes ambientes de execução.

Cada modo deverá possuir comportamento documentado, opções válidas, artefatos gerados e garantias. Builds de produção deverão priorizar otimização e segurança; builds de depuração deverão preservar informações úteis para diagnóstico.

### 18.3 Versionamento da linguagem

NML deverá possuir versionamento formal, com:
- identificação de versão;
- controle de recursos por versão;
- compatibilidade entre versões quando possível;
- migrações;
- avisos de obsolescência;
- ferramentas de compatibilidade.

Mudanças incompatíveis deverão ser identificadas e documentadas. O compilador deverá informar a versão exigida pelo documento ou projeto e rejeitar construções incompatíveis conforme as regras da versão.

### 18.4 Empacotamento e distribuição

A distribuição deverá oferecer:
- empacotamento;
- otimização;
- manifests;
- integridade;
- versionamento;
- configurações por ambiente;
- preparação para diferentes plataformas de hospedagem.

O pacote deverá ser reproduzível quando as entradas e configurações forem iguais. A especificação deverá definir metadados do pacote, dependências incluídas, recursos externos, hashes e estrutura final.

## 19. Diagnósticos e tratamento de erros

NML deverá distinguir erros de diferentes etapas:
- erros léxicos;
- erros sintáticos;
- erros semânticos;
- erros de tipagem;
- erros de compilação;
- erros de carregamento;
- erros de execução;
- erros de integração com APIs.

Os diagnósticos deverão ser estruturados e conter, quando disponível:
- código estável;
- categoria e severidade;
- mensagem em português;
- localização no arquivo;
- contexto relevante;
- explicação da causa;
- sugestão de correção;
- referências à regra violada.

O sistema deverá oferecer rastreamento, recuperação quando segura e integração com os diagnósticos de Nebula. A recuperação do parser não deverá mascarar erros nem produzir artefatos executáveis inválidos como se fossem corretos.

## 20. Especificação formal da linguagem

A especificação formal de NML deverá documentar, no mínimo:

1. conjunto de caracteres e codificação;
2. regras léxicas e tokens;
3. identificadores e palavras reservadas;
4. comentários e metadados;
5. gramática formal;
6. precedência e associatividade, quando aplicáveis;
7. estrutura documental e regras de aninhamento;
8. regras de parsing e construção da AST;
9. nós e invariantes da AST;
10. regras semânticas;
11. sistema de tipos;
12. elementos nativos e seus contratos;
13. componentes e composição;
14. expressões e integração com Nebula;
15. atributos e vinculações;
16. eventos e navegação;
17. modelo de estado;
18. ciclos de vida;
19. renderização e hidratação;
20. modelo de execução e permissões;
21. integração com APIs do navegador;
22. regras de compilação e formatos de saída;
23. diagnósticos e recuperação;
24. exemplos normativos e não normativos;
25. critérios de conformidade e testes de referência.

A gramática deverá ser suficientemente precisa para permitir implementações interoperáveis do lexer, parser, AST, análise semântica, compilador e ferramentas.

## 21. Arquitetura lógica de implementação

A implementação deverá manter responsabilidades separadas. A arquitetura lógica esperada inclui:

1. **Leitura de fonte:** entrada de arquivos, codificação e localização de origem.
2. **Análise léxica:** conversão de caracteres em tokens e diagnósticos léxicos.
3. **Análise sintática:** validação da gramática e construção da AST.
4. **AST:** representação tipada e estável da estrutura NML.
5. **Análise semântica:** validação de escopos, elementos, componentes, propriedades, tipos, módulos e permissões.
6. **Resolução de módulos e recursos:** resolução determinística de dependências e arquivos.
7. **Análise e otimização:** análise estática, tree shaking e transformações seguras.
8. **Planejamento de renderização:** determinação das estratégias de SSR, CSR, hidratação e atualização.
9. **Geração de saída:** produção de HTML interoperável, manifests e recursos de execução.
10. **Integração de runtime:** ligação com estado, eventos, navegação, APIs e módulos Nebula.
11. **Diagnósticos e source maps:** rastreabilidade entre fonte e artefatos.
12. **Testes e conformidade:** testes unitários, integração, E2E e validação da especificação.

Essa lista define separação de responsabilidades, não obriga que cada etapa seja um executável ou pacote independente. A organização física deverá ser definida pela arquitetura do repositório.

## 22. Interoperabilidade e limites da plataforma

NML deverá utilizar padrões da Web quando isso proporcionar compatibilidade e segurança. A linguagem não deverá reinventar protocolos, formatos ou ambientes padronizados sem necessidade técnica justificada.

WebAssembly poderá ser utilizado como alvo ou ambiente de interoperabilidade quando adequado, conforme a arquitetura geral do Nebula-Web. Isso não significa que todo documento NML precise ser convertido em WebAssembly. A saída principal de marcação deverá permanecer interoperável com os navegadores existentes.

Recursos dependentes de suporte específico deverão ser detectados. Quando não houver suporte, o runtime deverá aplicar fallback documentado ou emitir diagnóstico apropriado.

## 23. Critérios gerais de conformidade

Uma implementação não deverá ser declarada conforme apenas por aceitar exemplos simples. A conformidade deverá ser avaliada por categorias:

- **Conformidade léxica:** tokens e comentários de acordo com a especificação.
- **Conformidade sintática:** aceitação e rejeição corretas de documentos.
- **Conformidade semântica:** validação de tipos, escopos, componentes e regras.
- **Conformidade de execução:** comportamento definido para eventos, estado, renderização e ciclos de vida.
- **Conformidade de segurança:** aplicação das permissões e restrições.
- **Conformidade de interoperabilidade:** saídas e integrações compatíveis com os destinos declarados.
- **Conformidade de ferramentas:** diagnósticos, source maps e testes de referência.
- **Conformidade por versão:** comportamento consistente com a versão declarada.

Cada requisito deverá ser associado a testes verificáveis. Recursos ainda não implementados deverão ser identificados como pendentes, experimentais ou não suportados; não poderão ser anunciados como concluídos.

## 24. Decisões consolidadas da entrevista da Fase 1

Esta seção registra as 51 escolhas feitas durante a entrevista de requisitos. Os números representam as opções selecionadas, não prioridades nem níveis de implementação.

| Nº | Tema | Opção | Requisito consolidado |
|---:|---|:---:|---|
| 1 | Filosofia | 4 | Linguagem híbrida, com recursos de marcação e funcionalidades próprias. |
| 2 | Sintaxe | 3 | Sintaxe totalmente original. |
| 3 | Estrutura documental | 3 | Regras obrigatórias combinadas com liberdade estrutural. |
| 4 | Palavras-chave | 1 | Português, alinhado à identidade Nebula. |
| 5 | Integração NSL | 3 | Arquivos NSL separados e estilos internos em NML. |
| 6 | Estilos internos | 3 | Estilos locais e compartilháveis. |
| 7 | Hierarquia | 3 | Níveis obrigatórios combinados com níveis livres. |
| 8 | Tipagem | 3 | Regras rígidas para elementos nativos e maior flexibilidade controlada para personalizados. |
| 9 | Componentes personalizados | 4 | Componentes reutilizáveis integrados a funções e módulos Nebula. |
| 10 | Conteúdo dinâmico | 4 | Integração com expressões Nebula nos contextos autorizados. |
| 11 | Eventos | 4 | Eventos próprios, predefinidos e personalizados, integrados a Nebula. |
| 12 | Navegação | 4 | Rotas, parâmetros, histórico e navegação programática. |
| 13 | Multimídia | 4 | Imagens, vídeo, áudio, streaming, processamento e carregamento dinâmico. |
| 14 | Formulários | 4 | Validação, estados, eventos, envio assíncrono e campos personalizados. |
| 15 | Acessibilidade | 4 | Padrões, requisitos, validação e diagnóstico automatizado. |
| 16 | Comentários | 4 | Linha única, múltiplas linhas, documentação, condicionais e metadados. |
| 17 | Atributos | 4 | Atributos tipados, personalizados, dinâmicos, herdáveis e reativos. |
| 18 | Segurança | 4 | Isolamento, permissões granulares e proteção contra injeções e recursos indevidos. |
| 19 | Módulos | 4 | Importação, exportação, namespaces, dependências e versões. |
| 20 | Erros | 4 | Diagnósticos estruturados, rastreamento e recuperação nas etapas da linguagem. |
| 21 | Metadados | 4 | Metadados tradicionais e personalizados, incluindo SEO, segurança e execução. |
| 22 | Renderização | 4 | Renderização híbrida, reativa, incremental, SSR, CSR e hidratação. |
| 23 | SEO | 4 | SEO, dados estruturados, sitemap, controle de rastreamento e análise. |
| 24 | Compatibilidade | 4 | Camada de compatibilidade, detecção, fallbacks e polyfills. |
| 25 | Desempenho | 4 | Análise estática, tree shaking, divisão de código, cache e otimização. |
| 26 | Ferramentas | 4 | Depuração, inspeção, source maps, profiler, rede e integração com debugger Nebula. |
| 27 | Elementos nativos | 4 | Biblioteca completa, categorizada, documentada e extensível. |
| 28 | Manipulação estrutural | 4 | API reativa para criar, remover, modificar e observar elementos. |
| 29 | Estado | 4 | Estados locais, globais, derivados, persistentes, reativos e assíncronos. |
| 30 | Comunicação | 4 | HTTP, REST, WebSocket, SSE, streaming, cancelamento e interceptadores. |
| 31 | Armazenamento | 4 | Abstração para armazenamento local, sessão, IndexedDB, cookies e cache. |
| 32 | Organização de páginas | 4 | Páginas, layouts, rotas, subpáginas, documentos compartilhados e carregamento dinâmico. |
| 33 | Modelos | 4 | Modelos compostos, parâmetros, regiões substituíveis e layouts aninhados. |
| 34 | Condicionais e repetição | 4 | Condicionais, iteração, filtros, iteração assíncrona e otimização. |
| 35 | Vinculação de dados | 4 | Vinculação unidirecional, bidirecional, reativa, derivada e assíncrona. |
| 36 | Ciclo de vida de páginas | 4 | Inicialização, criação, montagem, atualização, suspensão, retomada, destruição e recuperação. |
| 37 | Ciclo de vida de componentes | 4 | Ciclos síncronos e assíncronos, efeitos, dependências e limpeza. |
| 38 | Controle de renderização | 4 | Atualização reativa e incremental, agendamento, priorização e controle manual. |
| 39 | Recuperação de erros | 4 | Isolamento, limites de erro, fallback, registro e recuperação configurável. |
| 40 | APIs do navegador | 4 | Integração tipada, permissões, detecção de compatibilidade e módulos Nebula. |
| 41 | Processamento paralelo | 4 | Workers, comunicação, filas, cancelamento e transferência de dados. |
| 42 | Segundo plano | 4 | Notificações, sincronização, tarefas dentro dos limites da plataforma e Service Workers. |
| 43 | Offline | 4 | Cache, sincronização, conectividade e suporte a PWA. |
| 44 | Gerenciamento de recursos | 4 | Catálogo, otimização, conversão, compressão, cache e versionamento. |
| 45 | Carregamento | 4 | Carregamento assíncrono, priorização, dependências, divisão e cancelamento. |
| 46 | Segurança de recursos externos | 4 | Políticas de origem, integridade, permissões e proteção de conteúdo. |
| 47 | Formatos de saída | 4 | HTML, recursos otimizados, metadados, manifests e artefatos por ambiente. |
| 48 | Modos de compilação | 4 | Desenvolvimento, produção, depuração, testes, análise e builds incrementais. |
| 49 | Versionamento | 4 | Versões formais, compatibilidade, migrações e obsolescência. |
| 50 | Distribuição | 4 | Empacotamento, integridade, versionamento e preparação para hospedagem. |
| 51 | Especificação formal | 4 | Gramática, tokens, AST, semântica, tipagem, execução, diagnósticos e conformidade. |

## 25. Itens que ainda exigem definição técnica

As escolhas da entrevista estabelecem o escopo desejado, mas não resolvem todas as decisões de engenharia. Antes de implementar a linguagem, documentos técnicos complementares deverão detalhar, sem presumir decisões ainda não tomadas:

- sintaxe concreta e exemplos normativos;
- gramática formal e estratégia de parsing;
- catálogo exato de elementos nativos;
- tipos, propriedades e contratos de cada elemento;
- modelo de escopo e resolução de nomes;
- formato e invariantes da AST;
- semântica de componentes, slots e modelos;
- integração formal com expressões e módulos Nebula;
- algoritmo de atualização reativa;
- regras de concorrência e cancelamento;
- contratos de APIs e permissões;
- estratégia de SSR, CSR e hidratação;
- matriz de navegadores e versões;
- formato dos manifests e pacotes;
- organização física dos módulos e ferramentas;
- política de compatibilidade e migração;
- critérios de desempenho e segurança mensuráveis.

Esses pontos deverão ser especificados em documentos derivados e decisões técnicas (ADRs), mantendo este documento como referência consolidada de requisitos.

## 26. Estado do documento

- **Entrevista de requisitos NML — Fase 1:** 51 perguntas respondidas.
- **Decisões registradas:** 51.
- **Especificação de requisitos:** consolidada neste documento.
- **Gramática normativa:** ainda precisa ser elaborada.
- **Implementação:** não é declarada concluída por este documento.
- **Testes de conformidade:** ainda precisam ser especificados e implementados.

Este arquivo é a referência de requisitos da NML no Nebula-Web. Alterações futuras deverão preservar o histórico das decisões e atualizar a versão deste documento quando houver mudança de escopo ou semântica.
