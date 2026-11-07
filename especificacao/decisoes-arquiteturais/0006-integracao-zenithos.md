# 0006 — Integração ZenithOS

O acesso a recursos de hardware será exclusivo do Portunus executado no ZenithOS. O adaptador usará a ABI/syscalls documentadas do kernel. O kernel mantém autoridade sobre validação, permissões, capacidades e drivers. Conteúdo Web e módulos comuns não poderão invocar syscalls arbitrárias.
