# Arquitetura geral

## Objetivo
Desenvolver um ecossistema para criar aplicações Web com Nebula, NML e NSL.

## Camadas
1. NML descreve estrutura e semântica de documentos.
2. NSL descreve apresentação e regras de estilo.
3. Compiladores independentes analisam e validam cada linguagem.
4. O pipeline coordena dependências e saídas.
5. O Runtime fornece APIs de execução e interoperabilidade com o host.
6. O backend WebAssembly gera módulos quando esse alvo for apropriado.
7. Adaptadores conectam o ecossistema a navegadores e, opcionalmente, ao Portunus/ZenithOS.

## Regra de implementação
Todo código-fonte próprio deve ser escrito em Nebula. Formatos declarativos, workflows e documentação usam seus formatos naturais. Ferramentas externas podem ser utilizadas no processo de build, mas não substituem implementações próprias em Nebula.

## Portabilidade
O núcleo não pode depender de ZenithOS. Recursos específicos do ZenithOS ficam isolados no adaptador correspondente.


---

## Contratos de fronteira NML–NSL–Nebula

A separação das linguagens é mantida em todos os estágios: NML e NSL possuem frontends, ASTs, semânticas e diagnósticos próprios. A integração ocorre por catálogo tipado de elementos/componentes, referências de estilo, classes, ids, estados, propriedades e nós de expressão Nebula explicitamente autorizados.

A especificação detalhada está em [CONTRATOS-NML-NSL-NEBULA.md](CONTRATOS-NML-NSL-NEBULA.md). O formato comum de diagnósticos e recuperação está em [DIAGNOSTICOS-E-RECUPERACAO.md](DIAGNOSTICOS-E-RECUPERACAO.md). Esses contratos são documentação draft e não comprovam implementação.
