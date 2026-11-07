# Contribuindo

## Regras fundamentais
1. Código-fonte próprio do Nebula-Web deve ser escrito em Nebula.
2. Não introduza implementação em Rust, C, C++, JavaScript ou outra linguagem como código-fonte do projeto. Artefatos gerados e arquivos de automação/configuração são exceções justificadas.
3. Preserve a independência de NML e NSL.
4. Toda funcionalidade deve incluir testes automatizados e documentação.
5. Não declare suporte ou conformidade sem testes reproduzíveis.
6. Mudanças na ABI ZenithOS devem ser coordenadas com o projeto ZenithOS e validadas contra a especificação oficial do kernel.

## Fluxo
- Abra uma issue para mudanças arquiteturais.
- Registre decisões relevantes em `especificacao/decisoes-arquiteturais/`.
- Use branches e pull requests pequenos e revisáveis.
- Atualize documentação, testes e changelog quando aplicável.
