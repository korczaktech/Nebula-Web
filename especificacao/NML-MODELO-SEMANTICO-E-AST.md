# NML — Modelo semântico, tipos e AST

**Versão:** 0.1.0-draft  
**Estado:** contrato técnico proposto

## 1. Objetivo

Definir o significado dos nós aceitos pela gramática NML, a estrutura pública da AST, validação semântica e invariantes para compilação determinística.

## 2. Modelo de documento

Um arquivo analisado produz §NmlDocument§ com versão de linguagem, cabeçalho opcional, metadados, regiões principais, regiões auxiliares, declarações de extensão, tabela de símbolos, diagnósticos e mapa de origem.

A ordem original é preservada. A ordem de renderização é determinada pelas regras de cada região, não por ordenação alfabética.

## 3. Nós públicos da AST

| Nó | Conteúdo essencial |
|---|---|
| NmlDocument | versão, cabeçalho, regiões e origem |
| DocumentHeader | linguagens declaradas e origem |
| RegionNode | tipo da região e filhos |
| ElementNode | nome resolvido, propriedades, filhos e origem |
| PropertyNode | nome, valor, tipo resolvido e origem |
| TextValueNode | conteúdo textual e origem |
| LiteralNode | string, número, booleano ou nulo |
| ListNode | sequência ordenada de valores |
| ObjectNode | pares chave-valor |
| ReferenceNode | caminho qualificado e símbolo resolvido |
| StyleReferenceNode | arquivo/módulo NSL associado |
| ExtensionNode | declaração namespaced |
| ErrorNode | nó sintético produzido somente em recuperação |

Todos os nós carregam arquivo, offsets de bytes, linha e coluna inicial/final. Colunas reportadas contam caracteres Unicode, não bytes.

## 4. Tipos de valores

Tipos iniciais: §Texto§, §Inteiro§, §Decimal§, §Logico§, §Nulo§, §Lista<T>§, §Objeto§, §Referencia<T>§ e tipos específicos do catálogo de elementos/propriedades.

Inferência ocorre pelo contexto da propriedade. Conversões implícitas são limitadas às conversões sem perda declaradas pelo tipo. Conversões destrutivas exigem conversão explícita prevista no catálogo.

## 5. Resolução de elementos

1. Resolver no catálogo nativo.
2. Se não existir, procurar em componentes importados e namespaces visíveis.
3. Mais de uma resolução gera erro de ambiguidade.
4. Nenhuma resolução gera erro de elemento desconhecido.
5. Componentes externos declaram contrato público de propriedades, conteúdo, eventos e estilos.
6. Extensões não podem redefinir silenciosamente elementos nativos.

Nomes como §div§, §p§, §t1§ e §titulo§ não se tornam automaticamente elementos nativos por aparecerem em exemplos; o catálogo oficial decide disponibilidade e semântica.

## 6. Regras de regiões

- §cabecalho§, §pagina§ e §rodape§ são opcionais e independentes.
- Cada região ocorre no máximo uma vez.
- §metadados§, §sistema§ e §estilo§ também são opcionais e únicos.
- Documento sem região principal é válido como unidade vazia; pode receber aviso de ausência de conteúdo.
- Ordem das regiões não altera sua identidade semântica.
- Conteúdo respeita o esquema de filhos da região.

## 7. Elementos e propriedades

- Elemento aceita somente propriedades declaradas em seu contrato.
- Propriedade duplicada é erro, salvo se marcada repetível.
- Propriedades obrigatórias são verificadas após resolução do elemento.
- Propriedade desconhecida é erro com sugestão quando possível.
- Filhos são validados segundo categorias permitidas pelo pai.
- Aninhamento livre não revoga restrições semânticas de conteúdo.
- §texto§ aceita valor textual e ocorre uma vez por elemento, salvo contrato específico.
- Texto não coexiste com filhos quando o contrato declara conteúdo exclusivo; elementos mistos declaram essa capacidade.
- §nulo§ só é aceito em propriedades anuláveis.

## 8. Acessibilidade

Elementos semânticos nativos declaram papel, nome acessível, estados e requisitos de teclado. O compilador detecta ausência de texto alternativo, rótulo de controle e relações semânticas quando verificável estaticamente. Componentes personalizados declaram mapeamento acessível ou recebem diagnóstico.

## 9. Segurança

- Referências são resolvidas sem executar conteúdo.
- URLs são validadas por esquema e política de origem.
- Conteúdo textual é texto, nunca HTML executável.
- Conteúdo não confiável exige API tipada de sanitização.
- NML não autoriza syscalls, hardware ou acesso arbitrário ao sistema.
- AST não contém nós de execução arbitrária.

## 10. Pipeline semântico

1. Validar versão e cabeçalho.
2. Construir escopos e namespaces.
3. Registrar regiões.
4. Resolver elementos/componentes.
5. Resolver propriedades e referências.
6. Inferir e validar tipos.
7. Validar estrutura pai-filho e conteúdo.
8. Validar acessibilidade e segurança.
9. Vincular referências NSL.
10. Validar nós de expressão Nebula somente em contextos autorizados.
11. Produzir AST validada ou parcial com erros.

AST com erros não pode ser marcada semanticamente válida. Ferramentas podem receber AST parcial para editor/recuperação.

## 11. Determinismo

Mesma entrada, versão de especificação e dependências produzem a mesma AST, símbolos, diagnósticos e ordem de referências. Resolução não depende da ordem não documentada do sistema de arquivos.

## 12. Fora do núcleo estático 0.1

Estado reativo, eventos, expressões Nebula, componentes assíncronos, SSR, hidratação e ciclo de vida exigem nós e contratos adicionais antes de integrarem a gramática estática.
