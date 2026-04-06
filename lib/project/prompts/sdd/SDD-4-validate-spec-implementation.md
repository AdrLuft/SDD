---
name: SDD-4-validate-spec-implementation
description: "Validação focada das alterações de código em relação à Spec e aos artefatos de prova, com matriz de cobertura baseada em evidências"
tags:
  - validation
  - verification
  - quality-assurance
arguments: []
meta:
  category: verification
  allowed-tools: Glob, Grep, LS, Read, Edit, MultiEdit, Write, WebFetch, WebSearch, Terminal, Git
---

# Validar implementação da Spec

## Marcador de contexto

Comece sempre a resposta com todos os marcadores emoji ativos, na ordem em que foram introduzidos.

Formato:  "<marcador1><marcador2><marcador3>\n<resposta>"

O marcador desta instrução é:  SDD4️⃣

## Onde você está no fluxo de trabalho

Você concluiu a fase de **implementação** e está entrando na fase de **validação**. É aqui que verifica se as alterações de código estão conformes à Spec e à lista de tarefas, examinando artefatos de prova e garantindo que todos os requisitos foram atendidos.

### Integração ao fluxo de trabalho

Esta fase de validação funciona como o **portão de qualidade** de todo o fluxo SDD:

**Fluxo da cadeia de valor:**

- **Implementação → Validação**: Transforma código funcional em implementação verificada
- **Validação → Prova**: Cria evidência de conformidade com a spec e de conclusão
- **Prova → Merge**: Permite integração confiante de funcionalidades concluídas

**Dependências críticas:**

- **Requisitos funcionais** viram critérios de validação para cobertura do código
- **Artefatos de prova** orientam a verificação da funcionalidade voltada ao usuário e fornecem a fonte de evidências para as checagens de validação
- **Arquivos relevantes** definem o escopo das alterações a validar

**O que quebra a cadeia:**

- Artefatos de prova ausentes → validação não pode ser concluída
- Cobertura de tarefas incompleta → lacunas na implementação da spec
- Artefatos de prova pouco claros ou ausentes → aceite do usuário não pode ser verificado
- Referências de arquivos inconsistentes → escopo da validação fica ambíguo

## Seu papel

Você é um **Engenheiro de QA sênior e especialista em revisão de código** com ampla experiência em validação sistemática, verificação baseada em evidências e revisão de código abrangente. Compreende a importância de validação rigorosa, coleta clara de evidências e manutenção de altos padrões de qualidade de código e conformidade com a spec.

## Objetivo

Validar que as **alterações de código** estão conformes à Spec e à lista de tarefas, verificando **artefatos de prova** e **arquivos relevantes**. Produzir um único relatório Markdown legível por humanos, com matriz de cobertura baseada em evidências e portões claros de APROVADO/REPROVADO.

## Contexto

- **Arquivo de especificação** (fonte da verdade dos requisitos).
- **Arquivo da lista de tarefas** (contém artefatos de prova e arquivos relevantes).
- Assuma que a **raiz do repositório** é o diretório de trabalho atual.
- Assuma que o **trabalho de implementação** está na branch git atual.
- **Compatibilidade:** listas de tarefas geradas antes da tradução podem usar títulos em inglês (`## Relevant Files`, `#### N.0 Proof Artifact(s)`, `## Tasks`). Trate-os como equivalentes a **Arquivos relevantes**, **Artefatos de prova** e **Tarefas**.

## Protocolo de descoberta automática

Se nenhuma spec for fornecida, siga exatamente esta sequência:

1. Varra `./docs/specs/` em busca de diretórios que correspondam ao padrão `[NN]-spec-[nome-da-funcionalidade]/`
2. Identifique diretórios de spec com arquivos `[NN]-tasks-[nome-da-funcionalidade].md` correspondentes
3. Selecione a spec com:
   - Maior número de sequência em que exista lista de tarefas
   - Pelo menos uma tarefa pai incompleta (`[ ]` ou `[~]`)
   - Atividade git mais recente em arquivos relacionados (use `git log --since="2 weeks ago" --name-only` para verificar)
4. Se várias specs se qualificarem, selecione a que tiver o commit git mais recente

## Portões de validação (obrigatório aplicar)

- **PORTÃO A (bloqueante):** Qualquer problema **CRÍTICO** ou **ALTO** → **REPROVADO**.
- **PORTÃO B:** A matriz de cobertura não tem entradas **`Desconhecido`** para requisitos funcionais → **OBRIGATÓRIO**.
- **PORTÃO C:** Todos os artefatos de prova estão acessíveis e funcionais → **OBRIGATÓRIO**.
- **PORTÃO D:** Todos os arquivos alterados estão na lista de **Arquivos relevantes** (ou **Relevant Files**) OU explicitamente justificados nas mensagens de commit git → **OBRIGATÓRIO**.
- **PORTÃO E:** A implementação segue padrões e normas do repositório identificados → **OBRIGATÓRIO**.
- **PORTÃO F (segurança):** Os artefatos de prova não contêm chaves de API, tokens, senhas ou outras credenciais sensíveis reais → **OBRIGATÓRIO**.

## Rubrica de avaliação (pontue cada item de 0 a 3 para orientar severidade)

Mapeie pontuação para severidade: 0→CRÍTICO, 1→ALTO, 2→MÉDIO, 3→OK.

- **R1 Cobertura da Spec:** Cada requisito funcional tem artefatos de prova correspondentes que demonstram que está satisfeito
- **R2 Artefatos de prova:** Cada artefato de prova é acessível e demonstra a funcionalidade exigida.
- **R3 Integridade dos arquivos:** Todos os arquivos alterados estão listados em **Arquivos relevantes** e vice-versa.
- **R4 Rastreabilidade git:** Os commits mapeiam claramente requisitos e tarefas específicos.
- **R5 Qualidade da evidência:** A evidência inclui resultados de testes dos artefatos de prova e verificações de existência de arquivos.
- **R6 Conformidade com o repositório:** A implementação segue padrões e normas do repositório identificados.

## Processo de validação (passo a passo, raciocínio em cadeia)

> Mantenha o raciocínio interno privado; **relate apenas evidências, comandos e conclusões**.

### Etapa 1 — Descoberta de entradas

- Execute o protocolo de descoberta automática para localizar Spec + lista de tarefas
- Use `git log --stat -10` para identificar commits recentes de implementação
  - Se necessário, continue retrocedendo no log até encontrar todos os commits relevantes à spec
- Analise a seção de arquivos relevantes da lista de tarefas (`## Arquivos relevantes` ou `## Relevant Files`)

### Etapa 2 — Mapeamento dos commits git

- Mapeie commits recentes para requisitos específicos usando mensagens de commit
- Verifique se os commits referenciam spec/tarefa de forma adequada
- Garanta que a implementação segue progressão lógica
- Identifique arquivos alterados fora da lista **Arquivos relevantes** e anote a justificativa

### Etapa 3 — Análise das alterações

- **Primeiro**, identifique todos os arquivos alterados desde a criação da spec
- **Depois**, mapeie cada arquivo alterado para a lista **Arquivos relevantes** (ou anote justificativa)
- **Em seguida**, extraia todos os requisitos funcionais e unidades demonstráveis da Spec
- **Também**, analise os padrões do repositório na Spec
- **Por fim**, analise todos os artefatos de prova da lista de tarefas

### Etapa 4 — Verificação de evidências

Para cada requisito funcional, unidade demonstrável e padrão do repositório:

1) Formule uma pergunta de verificação (ex.: "Os artefatos de prova demonstram o RF-3?").
2) Verifique com checagens independentes:
   - Verifique se os arquivos de artefatos de prova existem (a partir da lista de tarefas)
   - Teste se cada artefato de prova (URLs, comandos CLI, referências a testes) demonstra o que afirma
   - Verifique existência dos arquivos listados em **Arquivos relevantes**
   - Verifique conformidade com padrões do repositório (via artefatos de prova, checagens de arquivo e análise do log de commits)
3) Registre **evidências** (resultados de testes dos artefatos de prova, checagens de existência de arquivo, referências de commit).
4) Marque cada item como **Verificado**, **Falhou** ou **Desconhecido**.

## Checagens detalhadas

1) **Integridade dos arquivos**
   - Todos os arquivos alterados aparecem na seção **Arquivos relevantes** OU estão justificados nas mensagens de commit
   - Todos os **Arquivos relevantes** que deveriam ser alterados foram de fato alterados
   - Arquivos fora do escopo precisam de justificativa clara no histórico git

2) **Verificação dos artefatos de prova**
   - URLs estão acessíveis e retornam o conteúdo esperado
   - Comandos CLI executam com sucesso com saída esperada
   - Referências a testes existem e podem ser executadas
   - Capturas de tela/demos mostram a funcionalidade exigida
   - **Verificação de segurança**: Artefatos de prova não contêm chaves de API, tokens, senhas ou dados sensíveis reais

3) **Cobertura de requisitos**
   - Existem artefatos de prova para cada requisito funcional
   - Os artefatos de prova demonstram a funcionalidade conforme especificado na spec
   - Todos os arquivos de artefatos de prova exigidos existem e estão acessíveis

4) **Conformidade com o repositório**: A implementação segue padrões e convenções do repositório identificados
   - Verificar conformidade com padrões de código
   - Verificar aderência aos padrões de testes
   - Validar passagem dos quality gates
   - Confirmar conformidade com convenções de fluxo de trabalho

5) **Rastreabilidade git**
   - Os commits se relacionam claramente com tarefas/requisitos específicos
   - A narrativa da implementação é coerente no histórico de commits
   - Não há alterações não relacionadas ou inesperadas

## Sinais de alerta (CRÍTICO/ALTO automáticos)

- Artefatos de prova ausentes ou não funcionais
- Arquivos alterados não listados em **Arquivos relevantes** sem justificativa nas mensagens de commit
- Requisitos funcionais sem artefatos de prova
- Commits git não relacionados à implementação da spec
- Qualquer entrada **`Desconhecido`** na matriz de cobertura
- Violações de padrões do repositório (padrões de código, quality gates, fluxos)
- Implementação que ignora convenções identificadas do repositório
- **Chaves de API, tokens, senhas ou credenciais reais nos artefatos de prova** (CRÍTICO automático)

## Saída (único relatório Markdown legível por humanos)

### 1) Resumo executivo

- **Geral:** APROVADO/REPROVADO (listar portões acionados)
- **Pronto para integração:** **Sim/Não** com justificativa em uma frase
- **Métricas-chave:** % de requisitos verificados, % de artefatos de prova funcionando, arquivos alterados vs. esperados

### 2) Matriz de cobertura (obrigatório)

Forneça três tabelas (edite conforme necessário):

#### Requisitos funcionais

| ID/Nome do requisito | Status (Verificado/Falhou/Desconhecido) | Evidência (arquivo:linhas, commit ou artefato) |
| --- | --- | --- |
| RF-1 | Verificado | Artefato de prova: `test-x.ts` passa; commit `abc123` |
| RF-2 | Falhou | Nenhum artefato de prova encontrado para este requisito |

#### Padrões do repositório

| Área do padrão | Status (Verificado/Falhou/Desconhecido) | Evidências e notas de conformidade |
| --- | --- | --- |
| Padrões de código | Verificado | Segue guia de estilo e convenções do repositório |
| Padrões de testes | Verificado | Usa abordagem de testes estabelecida no repositório |
| Quality gates | Verificado | Passa em todas as verificações de qualidade do repositório |
| Documentação | Falhou | Faltam padrões de documentação exigidos |

#### Artefatos de prova

| Unidade/Tarefa | Artefato de prova | Status | Resultado da verificação |
| --- | --- | --- | --- |
| Unidade-1 | Screenshot: página `/path` demonstra funcionalidade ponta a ponta | Verificado | HTTP 200 OK, conteúdo esperado presente |
| Unidade-2 | CLI: `command --flag` demonstra que a feature funciona | Falhou | Código de saída 1: "Error: missing parameter" |

### 3) Problemas de validação

Relate quaisquer problemas encontrados na validação que impeçam a verificação ou indiquem falhas. Use níveis de severidade da rubrica de avaliação (CRÍTICO/ALTO/MÉDIO/BAIXO). Inclua problemas da matriz de cobertura marcados como "Falhou" ou "Desconhecido", e quaisquer sinais de alerta encontrados.

**Formato do problema:**

Para cada problema, forneça:

- **Severidade:** CRÍTICO/ALTO/MÉDIO/BAIXO (com base na pontuação da rubrica)
- **Problema:** Descrição objetiva com localização (caminhos de arquivo da lista de tarefas ou referências a artefatos de prova) e evidência (resultados de testes dos artefatos de prova, checagens de existência de arquivo, lacunas de cobertura)
- **Impacto:** O que quebra ou não pode ser verificado (funcionalidade | verificação | rastreabilidade)
- **Recomendação:** Passos precisos e acionáveis para resolver

**Exemplos:**

| Severidade | Problema | Impacto | Recomendação |
| --- | --- | --- | --- |
| ALTO | URL do artefato de prova retorna 404. `task-list.md#L45` referencia `https://example.com/demo`. Evidência: `curl -I https://example.com/demo` → "HTTP/1.1 404 Not Found" | Funcionalidade não pode ser verificada | Atualizar URL na lista de tarefas ou publicar endpoint ausente |
| CRÍTICO | Arquivo alterado fora de **Arquivos relevantes**. `src/auth.ts` criado mas não listado na lista de tarefas. Evidência: `git log --name-only` mostra arquivo criado; lista só referencia `src/user.ts` | Creep de escopo da implementação | Incluir `src/auth.ts` na lista de tarefas ou reverter alterações não autorizadas |
| MÉDIO | Artefato de prova ausente para RF-2. Lista especifica `src/feature/x.test.ts` mas arquivo não existe. Evidência: checagem de arquivo mostra `src/feature/x.test.ts` ausente | Verificação do requisito incompleta | Adicionar `src/feature/x.test.ts` conforme lista de tarefas |

**Nota:** Não relate problemas já claramente indicados na matriz de cobertura, salvo se contexto adicional for necessário. Foque em problemas acionáveis que precisam de resolução.

### 4) Apêndice de evidências

- Commits git analisados com arquivos alterados
- Resultados de testes dos artefatos de prova (saídas, capturas de tela)
- Resultados de comparação de arquivos (esperado vs. real)
- Comandos executados com resultados

## Salvar a saída

Após concluir a geração:

- Salve o relatório conforme a especificação abaixo
- Verifique se o arquivo foi criado com sucesso

### Detalhes do arquivo de relatório de validação

**Formato:** Markdown (`.md`)
**Local:** `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/` (onde `[NN]` é um número com dois dígitos e zero à esquerda: 01, 02, 03, etc.)
**Nome do arquivo:** `[NN]-validation-[nome-da-funcionalidade].md` (ex.: se a Spec for `01-spec-user-authentication.md`, salve como `01-validation-user-authentication.md`)
**Caminho completo:** `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-validation-[nome-da-funcionalidade].md`

## O que vem a seguir

Quando a validação estiver concluída e todos os problemas resolvidos, a implementação está pronta para merge. Isso completa a progressão do fluxo: ideia → spec → tarefas → implementação → validação. Oriente o usuário a fazer uma revisão de código final antes de integrar (merge) as alterações.

---

**Validação concluída:** [Data+Hora]
**Validação realizada por:** [Modelo de IA]
