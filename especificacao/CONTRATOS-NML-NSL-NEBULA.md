# Contratos de integração — NML, NSL e Nebula

**Versão:** 0.1.0-draft  
**Estado:** contratos arquiteturais propostos  
**Implementação pretendida:** 100% em Nebula

## 1. Independência

NML e NSL permanecem linguagens distintas. Cada uma possui tokens, gramática, lexer, parser, AST, semântica, diagnósticos, compilador e corpus próprios. Compartilhar infraestrutura não funde gramáticas. Nebula é a linguagem de implementação e integração; expressões Nebula entram somente por contextos autorizados.

## 2. Contratos de fronteira

| Origem | Destino | Contrato |
|---|---|---|
| NML | NSL | referências de arquivo/módulo, identidade de componente, classe, id, estados e escopo |
| NSL | NML | seletores validados contra catálogo de elementos, classes, ids e estados |
| NML | Nebula | nós de expressão tipados, propriedades, eventos, parâmetros e referências |
| NSL | Nebula | funções de valor autorizadas, tokens dinâmicos e dependências reativas tipadas |
| Nebula | NML | tipos públicos, funções, módulos, componentes e contratos de dados |
| Nebula | NSL | tipos de valores, funções puras autorizadas, tokens e fontes de estado |

## 3. Contrato NML–NSL

### 3.1 Identidade de elemento

Cada elemento resolvido recebe identidade semântica estável: nome/tipo, namespace e versão, id local, classes, atributos públicos tipados, componente proprietário e escopo. A identidade não depende da posição incidental do nó.

### 3.2 Validação cruzada

1. Analisar NML e NSL separadamente.
2. Construir catálogo de elementos/componentes, classes e ids.
3. Resolver referências de estilo.
4. Validar seletores contra catálogo disponível.
5. Verificar estados e propriedades expostos.
6. Emitir diagnósticos com origem correta.
7. Preservar compilação isolada quando integração não for necessária.

Seletor sem correspondência é aviso por padrão; perfil estrito pode convertê-lo em erro. A política deve estar no manifesto.

### 3.3 Escopos

Escopos global, módulo, componente e local são distintos. Estilos locais não escapam do componente por coincidência de nome. Exportação exige declaração explícita.

### 3.4 Classes e ids

NML declara classes e ids como metadados estruturais tipados. NSL usa §.nome§ para classe e §nome§ para elemento. A extensão proposta §#nome§ seleciona id único. Classes podem ser múltiplas; ids são únicos no escopo documental. Normalização NFC é comum, aplicada independentemente por cada lexer.

## 4. Contrato NML–Nebula

- A gramática NML não aceita trechos arbitrários de Nebula.
- Expressões são nós opacos tipados cuja fonte é delegada ao parser Nebula oficial.
- Cada contexto declara tipos de entrada/saída, efeitos, sincronismo e permissões.
- Expressões de conteúdo produzem tipo convertível ao conteúdo do elemento.
- Expressões em propriedades satisfazem o tipo da propriedade.
- Eventos chamam funções compatíveis com a assinatura do evento.
- Interpolação textual não pode contornar validação de tipos.
- Erros do parser Nebula são encaminhados com localização mapeada ao arquivo NML.

## 5. Contrato NSL–Nebula

- Funções em valores NSL devem ser puras ou declarar efeitos permitidos.
- Funções declaram tipo de retorno compatível com propriedade.
- Dependências reativas formam grafo acíclico ou reportam ciclos com caminho.
- Funções não modificam seletor, escopo ou política de segurança.
- Valores dinâmicos possuem fallback quando renderização estática for necessária.

## 6. Pipeline de compilação

1. Carregar fontes e manifesto.
2. Validar codificação e versão.
3. Executar lexers independentes.
4. Executar parsers independentes.
5. Construir ASTs independentes.
6. Resolver módulos e símbolos.
7. Validar semântica individual.
8. Construir catálogo compartilhado.
9. Validar referências NML–NSL.
10. Validar expressões Nebula em contextos autorizados.
11. Produzir IR de interface/estilo com referências tipadas.
12. Validar IR e gerar artefatos.

Nenhuma etapa trata texto de uma linguagem como código da outra sem passar pelo parser correspondente.

## 7. Diagnósticos e origem

Todo diagnóstico contém código estável, severidade, mensagem, arquivo e intervalo, linguagem responsável, etapa, notas relacionadas, sugestão opcional e causa/cadeia de dependência. Diagnósticos cruzados apontam referência e declaração incompatível.

## 8. Segurança e plataformas

- Documentos NML/NSL não possuem acesso direto a syscalls, hardware, memória privilegiada ou sistema operacional.
- APIs de navegador exigem permissões e contratos tipados.
- Integração ZenithOS é restrita ao Portunus executando no ZenithOS e mediada pelo kernel via ABI/syscalls autorizadas.
- Nenhuma capacidade especial torna-se disponível em navegador comum.
- Plugins/extensões têm sandbox, permissões e limites de recursos.

## 9. Versionamento

Cada linguagem publica versão própria. O contrato integrado declara intervalos compatíveis NML/NSL/Nebula. Incompatibilidade é detectada antes de gerar artefatos. Mudança incompatível exige major.

## 10. Critérios de aceitação

- Compilação isolada de NML e NSL não carrega o parser da outra linguagem.
- Compilação integrada resolve classes, elementos, ids, componentes, propriedades e estados.
- Referências inválidas produzem diagnósticos estáveis.
- Expressões Nebula só aparecem em contextos autorizados.
- Nenhum caminho contorna validação de tipos ou segurança.
- Source maps atravessam fronteiras entre linguagens.
- Testes diferenciais verificam equivalência esperada entre compilação isolada e integrada.


## 11. Resolução cruzada normativa

### 11.1 Identidade e catálogo

A identidade de elemento NML é a tupla (namespace, nome normalizado NFC, versão do catálogo, identidade do componente). Classes e ids pertencem ao nó NML e são resolvidos dentro do escopo documental/componente. Classes podem repetir-se; id deve ser único no escopo documental, salvo escopo isolado explicitamente declarado pelo contrato de componente.

O catálogo declara: nome e versão, tipo de nó, propriedades, tipos, nulabilidade, obrigatoriedade, repetibilidade, modelo de conteúdo, classes/estados expostos, atributos selecionáveis, partes, acessibilidade e capacidades. Nomes não catalogados não são implicitamente nativos.

### 11.2 Resolução de seletores

NSL resolve seletor contra o catálogo NML disponível no projeto. Se NML for compilada isoladamente, seletores podem ser analisados sem resolução cruzada; o resultado é AST não vinculada. Em compilação integrada, elemento, classe, id, atributo, estado e parte são verificados. Seletor válido sem correspondência gera aviso; incompatibilidade estrutural ou tentativa de atravessar escopo isolado gera erro.

### 11.3 Expressões Nebula

O núcleo NML 0.1.0 e NSL 0.2.0 não aceitam trechos arbitrários de Nebula. Uma extensão futura deve definir nó de expressão opaco, delimitador, contexto, tipo de entrada/saída, efeitos permitidos, sincronismo, permissões e mapeamento de origem antes de aceitar qualquer expressão. O parser correspondente é sempre o parser oficial Nebula.

### 11.4 Pipeline e falhas

Compilação integrada executa lexers/parsers independentes, semântica individual, resolução de módulos, construção do catálogo, vínculo NML–NSL, validação de tipos e geração de IR. Falha em um frontend não pode ser mascarada por fallback do outro. AST parcial é somente para ferramentas e nunca artefato válido.

### 11.5 Compatibilidade

Cada linguagem possui versão independente. O manifesto declara versões exatas/intervalos compatíveis e versões do catálogo. Ausência ou incompatibilidade de versão é erro antes da geração. Source maps preservam spans e origem ao atravessar fronteiras.
