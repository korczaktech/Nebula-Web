# Roadmap

## Etapa 0 — Fundação
- [x] Criar estrutura inicial do repositório.
- [ ] Aprovar arquitetura e contratos públicos.
- [ ] Definir especificações versionadas.

## Etapa 1 — NML
- [ ] Sintaxe e gramática.
- [ ] Lexer, Parser, AST e semântica.
- [ ] Compilador e geração de HTML.
- [ ] Testes de conformidade e documentação.

## Etapa 2 — NSL
- [ ] Sintaxe e gramática.
- [ ] Lexer, Parser, AST e semântica.
- [ ] Cascata, seletores e compilador.
- [ ] Geração de CSS, testes e documentação.

## Etapa 3 — Runtime e backend
- [ ] Contratos do Runtime.
- [ ] APIs de host e segurança.
- [ ] Backend WebAssembly e interoperabilidade.

## Etapa 4 — Integrações e ferramentas
- [ ] Integração com o compilador Nebula.
- [ ] Navegadores compatíveis.
- [ ] Adaptador Portunus/ZenithOS.
- [ ] CLI, debugger, LSP, autocomplete e servidor de desenvolvimento.

## Etapa 5 — Estabilização
- [ ] Testes ponta a ponta.
- [ ] Segurança, desempenho e regressão.
- [ ] Documentação final e releases.


---

## Estado documental atualizado — formalização NML/NSL

A documentação inicial de formalização foi elaborada, mas ainda está em revisão draft. Os checkboxes de implementação permanecem desmarcados intencionalmente.

### NML — entregáveis documentais
- [x] Proposta léxica e sintática 0.1.0-draft.
- [x] Gramática EBNF do núcleo estático 0.1.0-draft.
- [x] Modelo semântico, tipos e contrato de AST em documentação.
- [x] Corpus positivo/negativo especificado.
- [ ] Revisão normativa e aprovação formal.
- [ ] Execução do corpus por implementação de referência.
- [ ] Lexer, parser e AST implementados em Nebula.

### NSL — entregáveis documentais
- [x] Proposta léxica e sintática consolidada.
- [x] Revisão EBNF 0.2.0-draft para seletores diretos.
- [x] Modelo semântico, tipos, especificidade, cascata e AST documentados.
- [x] Corpus positivo/negativo ampliado.
- [ ] Revisão normativa e aprovação formal.
- [ ] Execução do corpus por implementação de referência.
- [ ] Lexer, parser e AST implementados em Nebula.

### Integração e qualidade documental
- [x] Contratos NML–NSL–Nebula documentados.
- [x] Corpus de integração NML–NSL–Nebula especificado.
- [x] Códigos e envelope de diagnósticos documentados.
- [x] Envelope comum de diagnósticos e recuperação documentado.
- [ ] Testes de integração executados.
- [ ] Source maps ponta a ponta implementados e validados.

Os itens marcados [x] acima significam somente que o artefato documental foi criado. Não significam código, testes ou conformidade implementados.
