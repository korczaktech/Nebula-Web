# Segurança

Relate vulnerabilidades de forma privada aos mantenedores do projeto. Não publique detalhes exploráveis antes da correção.

## Princípios
- Conteúdo Web não recebe acesso direto a syscalls.
- O kernel ZenithOS é a autoridade final para permissões e operações de hardware.
- A integração Portunus/ZenithOS deve validar identidade do processo, capacidades, argumentos e limites de recursos.
- APIs de host devem expor somente capacidades explicitamente concedidas.
- Módulos WebAssembly devem ser validados antes da execução.
