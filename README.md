# Nebula-Web

Ecossistema Web oficial do Nebula: NML (Nebula Markup Language), NSL (Nebula Styling Language), compiladores, ferramentas, bibliotecas, Runtime Web e integrações.

## Princípios

- Todo código-fonte próprio será escrito em Nebula.
- NML e NSL terão especificações, compiladores e testes independentes.
- A infraestrutura compartilhada não poderá acoplar indevidamente as semânticas das linguagens.
- A saída deverá interoperar com HTML, CSS e APIs padronizadas dos navegadores.
- WebAssembly será utilizado como alvo/ambiente quando adequado; o projeto não pretende reinventar o padrão.
- A integração especial com ZenithOS será exclusiva do Portunus executado no ZenithOS e passará pela ABI/syscalls oficiais do kernel, com autorização e mediação do kernel.
- Páginas e conteúdo Web nunca terão acesso arbitrário a syscalls ou hardware.

## Estrutura

- `linguagens/NML`: linguagem de marcação.
- `linguagens/NSL`: linguagem de estilos.
- `compartilhado`: infraestrutura comum.
- `compilador`: pipeline de compilação.
- `runtime`: Runtime Web.
- `backend/webassembly`: geração e interoperabilidade Wasm.
- `integracoes`: Nebula, navegadores, Portunus e ZenithOS.
- `ferramentas`: CLI, formatador, linter, debugger e Language Server.
- `bibliotecas`: bibliotecas padrão e componentes.
- `testes`: testes transversais.

## Estado

Este commit estabelece a estrutura arquitetural inicial. Diretórios reservados não indicam que componentes já estejam implementados ou funcionais. Cada subsistema deverá receber especificação, implementação em Nebula e testes antes de ser considerado concluído.

Consulte `CONTRIBUTING.md`, `especificacao/arquitetura-geral.md` e `especificacao/roadmap.md`.
