# Modelo de segurança

- Isolamento entre documentos e origens.
- Política explícita para rede, armazenamento e APIs de host.
- Validação de módulos WebAssembly.
- Limites de memória, execução e recursos.
- Nenhum acesso direto de páginas a syscalls.
- O kernel ZenithOS valida e autoriza toda operação privilegiada solicitada pelo Portunus.
