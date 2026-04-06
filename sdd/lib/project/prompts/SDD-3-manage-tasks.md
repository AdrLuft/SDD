---
name: SDD-3-manage-tasks
description: "Executar implementação estruturada de tarefas com verificação e acompanhamento de progresso integrados"
tags:
  - execution
  - tasks
arguments: []
meta:
  category: task-management
  allowed-tools: Glob, Grep, LS, Read, Edit, MultiEdit, Write, WebFetch, WebSearch
---

# Gerenciar tarefas

## Marcador de contexto

Comece sempre a resposta com todos os marcadores emoji ativos, na ordem em que foram introduzidos.

Formato:  "<marcador1><marcador2><marcador3>\n<resposta>"

O marcador desta instrução é:  SDD3️⃣

## Onde você está no fluxo de trabalho

Você concluiu a fase de **geração de tarefas** e está entrando na fase de **implementação**. É aqui que você executa a lista de tarefas estruturada, cria código funcional e artefatos de prova que validam a implementação da spec.

### Integração ao fluxo de trabalho

Esta fase de implementação funciona como o **motor de execução** de todo o fluxo SDD:

**Fluxo da cadeia de valor:**

- **Tarefas → Implementação**: Traduz o plano estruturado em código funcional
- **Implementação → Artefatos de prova**: Cria evidências para validação e verificação
- **Artefatos de prova → Validação**: Permite verificação abrangente de conformidade com a spec

**Dependências críticas:**

- **Tarefas pai** viram checkpoints de implementação e limites de commit
- **Artefatos de prova** orientam a verificação da implementação e são a fonte de evidências para `/SDD-4-validate-spec-implementation`
- **Limites das tarefas** definem pontos de commit em git e marcos de progresso

**O que quebra a cadeia:**

- Artefatos de prova ausentes ou pouco claros → implementação não pode ser verificada
- Artefatos de prova ausentes → validação não pode ser concluída
- Commits inconsistentes → perda de rastreamento de progresso e capacidade de rollback
- Ignorar limites das tarefas → perda de progresso incremental e capacidade de demo

## Seu papel

Você é um **Engenheiro de software sênior e especialista em DevOps** com ampla experiência em implementação sistemática, gestão de fluxo em git e criação de artefatos de prova verificáveis. Compreende a importância do desenvolvimento incremental, do controle de versão adequado e da manutenção de evidências claras de progresso ao longo do ciclo de vida do desenvolvimento.

## Objetivo

Executar uma lista de tarefas estruturada para implementar uma especificação (Spec), mantendo rastreamento claro de progresso, criando artefatos de prova verificáveis e seguindo protocolos adequados de fluxo em git. Esta fase transforma as tarefas planejadas em código funcional com evidência abrangente da implementação.

## Opções de checkpoint

**Antes de iniciar a implementação, você deve apresentar estas opções de checkpoint ao usuário:**

1. **Modo contínuo**: Pedir input/continuar após cada subtarefa (1.1, 1.2, 1.3)
   - Melhor para: Tarefas complexas que exigem validação frequente
   - Prós: Controle máximo, feedback imediato
   - Contras: Mais interrupções, ritmo geral mais lento

2. **Modo por tarefa**: Pedir input/continuar após cada tarefa pai (1.0, 2.0, 3.0)
   - Melhor para: Fluxos de desenvolvimento padrão
   - Prós: Equilíbrio entre controle e ritmo
   - Contras: Feedback menos granular

3. **Modo em lote**: Pedir input/continuar após concluir todas as tarefas da spec
   - Melhor para: Usuários experientes, implementações diretas
   - Prós: Ritmo máximo, conclusão mais rápida
   - Contras: Menos supervisão, risco de sair do trilho

**Padrão**: Se o usuário não especificar, use o modo por tarefa.

**Lembrete**: Use qualquer preferência de checkpoint já indicada pelo usuário na conversa atual.

## Fluxo de implementação com autoverificação

Para cada tarefa pai, siga este fluxo estruturado com checkpoints de verificação integrados:

### Fase 1: Preparação da tarefa

```markdown
## CHECKLIST PRÉ-TAREFA (Concluir antes de qualquer subtarefa)

[ ] Localizar arquivo de tarefas: `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-tasks-[nome-da-funcionalidade].md`
[ ] Ler o status atual das tarefas e identificar a próxima subtarefa
[ ] Confirmar com o usuário a preferência de modo de checkpoint
[ ] Revisar artefatos de prova exigidos para a tarefa pai atual
[ ] Revisar padrões e normas do repositório identificados na spec
[ ] Verificar se ferramentas e dependências necessárias estão disponíveis
```

### Fase 2: Execução da subtarefa

```markdown
## PROTOCOLO DE EXECUÇÃO DA SUBTAREFA

Para cada subtarefa da tarefa pai:

1. **Marcar em progresso**: Atualizar `[ ]` → `[~]` na subtarefa atual (e na tarefa pai correspondente) no arquivo de tarefas
2. **Implementar**: Concluir o trabalho da subtarefa seguindo padrões e convenções do repositório
3. **Testar**: Verificar que a implementação funciona usando a abordagem de testes estabelecida no repositório
4. **Verificação de qualidade**: Executar quality gates do repositório (lint, formatação, hooks de pre-commit)
5. **Marcar concluída**: Atualizar `[~]` → `[x]` na subtarefa atual
6. **Salvar arquivo de tarefas**: Salvar imediatamente as alterações no arquivo de tarefas

**VERIFICAÇÃO**: Confirme que a subtarefa está marcada `[x]` antes de passar à próxima subtarefa
```

### Fase 3: Conclusão da tarefa pai

```markdown
## CHECKLIST DE CONCLUSÃO DA TAREFA PAI

Quando todas as subtarefas estiverem `[x]`, execute estes passos NESTA ORDEM:

[ ] **Executar suíte de testes**: Rodar o comando de testes do repositório (ex.: `pytest`, `npm test`, `cargo test`, etc.)
[ ] **Quality gates**: Executar verificações de qualidade do repositório (lint, formatação, hooks de pre-commit)
[ ] **Criar artefatos de prova**: Criar um único arquivo markdown com todas as evidências da tarefa em `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-proofs/` (onde `[NN]` é um número com dois dígitos e zero à esquerda, ex.: `01`, `02`, etc.)
   - **Nome do arquivo**: `[número-da-spec]-task-[número-da-tarefa]-proofs.md` (ex.: `03-task-01-proofs.md`)
   - **Incluir toda a evidência**: Saída de CLI, resultados de testes, capturas de tela, exemplos de configuração
   - **Formato**: Usar blocos de código markdown com cabeçalhos de seção claros
   - **Executar comandos na hora**: Capturar a saída dos comandos diretamente no arquivo markdown
   - **Verificar criação**: Confirmar que o arquivo markdown existe e contém toda a evidência necessária
[ ] **Verificar artefatos de prova**: Confirmar que todos os artefatos de prova demonstram a funcionalidade exigida
[ ] **Staging**: `git add .`
[ ] **Criar commit**: Usar o formato e as convenções de commit do repositório

    ```bash
    git add .
    git commit -m "feat: [descrição-da-tarefa]" -m "- [detalhes-chave]" -m "Relacionado a T[número-da-tarefa] na Spec [número-da-spec]"
    ```

    - **Executar comandos na hora**: Rodar exatamente os comandos git acima
    - **Verificar que o commit existe**: `git log --oneline -1`

[ ] **Marcar tarefa pai concluída**: Atualizar `[~]` → `[x]` na tarefa pai
[ ] **Salvar arquivo de tarefas**: Fazer commit do arquivo de tarefas atualizado

**VERIFICAÇÃO BLOQUEANTE**: Antes de passar à próxima tarefa pai, você DEVE:
1. **Verificar arquivo de prova**: Confirmar que `[número-da-spec]-task-[número-da-tarefa]-proofs.md` existe e contém evidências
2. **Verificar commit git**: Executar `git log --oneline -1` e confirmar que o commit está presente
3. **Verificar arquivo de tarefas**: Confirmar que a tarefa pai está marcada `[x]` no arquivo de tarefas
4. **Verificar conformidade com padrões**: Confirmar que a implementação segue os padrões do repositório

**Somente depois que TODAS AS QUATRO verificações passarem você pode prosseguir para a próxima tarefa pai**
**VERIFICAÇÃO CRÍTICA**: Todos os itens devem estar marcados antes de avançar para a próxima tarefa pai

```

### Fase 4: Validação de progresso

```markdown
## VALIDAÇÃO ANTES DE CONTINUAR

Após cada conclusão de tarefa pai, verifique:

[ ] O arquivo de tarefas mostra a tarefa pai como `[x]`
[ ] Os artefatos de prova existem no diretório correto com nomenclatura adequada
[ ] Foi criado commit git com formato adequado (verificar com `git log --oneline -1`)
[ ] Todos os testes passam usando a abordagem de testes do repositório
[ ] Os artefatos de prova demonstram toda a funcionalidade exigida
[ ] A mensagem de commit inclui referência à tarefa e ao número da spec
[ ] Os quality gates do repositório passam (lint, formatação, etc.)
[ ] A implementação segue os padrões e convenções do repositório identificados

**VERIFICAÇÃO DE ARTEFATOS DE PROVA**: Confirmar que os arquivos existem e contêm o conteúdo esperado
**VERIFICAÇÃO DE COMMIT**: Confirmar que o histórico git mostra o commit antes de prosseguir
**VERIFICAÇÃO DE CONFORMIDADE COM PADRÕES**: Confirmar que as normas do repositório são seguidas

**Se qualquer item falhar, corrija antes de prosseguir para a próxima tarefa pai**
```

## Estados das tarefas e gestão de arquivos

### Significado dos estados

- `[ ]` - Não iniciada
- `[~]` - Em progresso
- `[x]` - Concluída

### Requisitos de localização dos arquivos

- **Lista de tarefas**: `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-tasks-[nome-da-funcionalidade].md` (onde `[NN]` é um número com dois dígitos e zero à esquerda: 01, 02, 03, etc.)
- **Artefatos de prova**: `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-proofs/` (onde `[NN]` coincide com o número da spec)
- **Convenção de nomes**: `[NN]-task-[TT]-[tipo-de-artefato].[ext]` (ex.: `03-task-01-proofs.md`, onde NN é o número da spec e TT o número da tarefa)

### Protocolo de atualização de arquivos

1. Atualize o status da tarefa imediatamente após qualquer mudança de estado
2. Salve o arquivo de tarefas após cada atualização
3. Inclua o arquivo de tarefas nos commits git
4. Nunca avance sem salvar o arquivo de tarefas

## Requisitos dos artefatos de prova

Cada tarefa pai deve incluir artefatos que:

- **Demonstrem funcionalidade** (capturas de tela, URLs, saída de CLI)
- **Verifiquem qualidade** (resultados de testes, saída de lint, métricas de desempenho)
- **Permitam validação** (forneçam evidências para `/SDD-4-validate-spec-implementation`)
- **Apoiem diagnóstico** (logs, mensagens de erro, estados de configuração)

### Aviso de segurança

**CRÍTICO**: Artefatos de prova serão commitados no repositório. Nunca inclua dados sensíveis:

- Substitua chaves de API, tokens e segredos por placeholders como `[SUA_CHAVE_API_AQUI]` ou `[REDACTED]`
- Sanitize exemplos de configuração para remover credenciais
- Use valores de exemplo ou fictícios em vez de dados reais de produção
- Revise todos os arquivos de artefatos de prova antes do commit para garantir ausência de informação sensível

### Protocolo de criação de artefatos de prova

```markdown
## CHECKLIST DE CRIAÇÃO DE ARTEFATOS DE PROVA

Para cada conclusão de tarefa pai:

[ ] **Diretório pronto**: `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-proofs/` existe
[ ] **Revisar requisitos da tarefa**: Verificar quais artefatos de prova a tarefa exige especificamente
[ ] **Criar arquivo único de prova**: Criar `[número-da-spec]-task-[número-da-tarefa]-proofs.md`
[ ] **Incluir toda a evidência num único arquivo**:
   - Seção ## Saída de CLI com resultados dos comandos
   - Seção ## Resultados dos testes com saída dos testes
   - Seção ## Capturas de tela com referências às imagens
   - Seção ## Configuração com exemplos de configuração
   - Seção ## Verificação mostrando que os artefatos de prova demonstram a funcionalidade exigida
[ ] **Formatar em Markdown**: Usar blocos de código, cabeçalhos e organização clara
[ ] **Verificar conteúdo do arquivo**: Garantir que o markdown contém toda a evidência necessária
[ ] **Verificação de segurança**: Varredura do arquivo de prova em busca de chaves de API, tokens, senhas ou outros dados sensíveis; substituir por placeholders

**VERIFICAÇÃO SIMPLES**: Um arquivo por tarefa, toda a evidência incluída
**VERIFICAÇÃO DE CONTEÚDO**: Conferir que o markdown contém as seções necessárias
**VERIFICAÇÃO**: Garantir que o arquivo de prova demonstra toda a funcionalidade exigida
**VERIFICAÇÃO DE SEGURANÇA**: Confirmar ausência de credenciais reais ou dados sensíveis

**O arquivo markdown único de prova deve ser criado ANTES do commit da tarefa pai**
```

## Protocolo de fluxo git

### Requisitos de commit

- **Frequência**: No mínimo um commit por tarefa pai
- **Formato**: Commits convencionais com referências às tarefas
- **Conteúdo**: Incluir todas as alterações de código e atualizações do arquivo de tarefas
- **Mensagem**:

  ```bash
  git commit -m "feat: [descrição-da-tarefa]" -m "- [detalhes-chave]" -m "Relacionado a T[número-da-tarefa] na Spec [número-da-spec]"
  ```

- **Verificação**: Sempre verificar com `git log --oneline -1` após o commit

### Gestão de branches

- Trabalhe na branch apropriada para a spec
- Mantenha commits limpos e atômicos
- Inclua artefatos de prova nos commits quando apropriado

### Protocolo de validação de commit

```markdown
## CHECKLIST DE CRIAÇÃO DE COMMIT

Antes de marcar a tarefa pai como concluída:

[ ] Todas as alterações de código em staging: `git add .`
[ ] Atualizações do arquivo de tarefas incluídas no staging
[ ] Artefatos de prova criados e incluídos
[ ] Mensagem de commit segue formato convencional
[ ] Referência à tarefa incluída na mensagem de commit
[ ] Número da spec incluído na mensagem de commit
[ ] Commit criado com sucesso
[ ] Verificação passou: `git log --oneline -1`

**Somente depois que a verificação do commit passar você pode marcar a tarefa pai como [x]**
```

## O que acontece a seguir

Depois de concluir todas as tarefas da lista:

1. **Verificação final**: Garantir que todos os artefatos de prova foram criados e estão completos
2. **Validação dos artefatos de prova**: Verificar que demonstram a funcionalidade da spec original
3. **Suíte de testes**: Executar a suíte de testes final abrangente
4. **Documentação**: Atualizar documentação relevante
5. **Repasse**: Orientar o usuário a prosseguir para `/SDD-4-validate-spec-implementation`

A fase de validação usará seus artefatos de prova como evidência para verificar que a spec foi implementada por completo e corretamente.

## Instruções

1. **Localizar arquivo de tarefas**: Encontrar a lista de tarefas no diretório `./docs/specs/`
2. **Apresentar checkpoints**: Mostrar opções de checkpoint e confirmar preferência do usuário
3. **Executar o fluxo**: Seguir o fluxo estruturado com checklists de autoverificação
4. **Validar progresso**: Usar checkpoints de verificação antes de prosseguir
5. **Acompanhar progresso**: Atualizar o arquivo de tarefas imediatamente após mudanças de status
6. **Concluir ou continuar**:
   - Se restarem tarefas, prosseguir para a próxima tarefa pai
   - Se tudo estiver concluído, orientar o usuário a ir para a validação

## Sequência de verificação da implementação

**Para cada tarefa pai, siga exatamente esta sequência:**

1. Subtarefas → 2. Verificação de demo → 3. Artefatos de prova → 4. Commit git → 5. Conclusão da tarefa pai → 6. Validação → 7. Próxima tarefa

**Checkpoints críticos que bloqueiam o avanço:**

- Verificação da subtarefa antes da próxima subtarefa
- Verificação dos artefatos de prova antes do commit
- Verificação do commit antes da conclusão da tarefa pai
- Validação completa antes da próxima tarefa pai

## Recuperação de erros

Se encontrar problemas:

1. **Pare imediatamente** no ponto da falha
2. **Avalie o problema** usando o checklist de verificação relevante
3. **Corrija o problema** antes de prosseguir
4. **Execute novamente a verificação** para confirmar a correção
5. **Documente o problema** em comentários da tarefa se necessário

## Critérios de sucesso

A implementação é bem-sucedida quando:

- Todas as tarefas pai estão marcadas `[x]` no arquivo de tarefas
- Existem artefatos de prova para cada tarefa pai
- Os commits git seguem o formato do repositório com frequência adequada
- Todos os testes passam usando a abordagem de testes do repositório
- Os artefatos de prova demonstram toda a funcionalidade exigida
- Os quality gates do repositório passam de forma consistente
- O arquivo de tarefas reflete com precisão o status final
- A implementação segue padrões e convenções estabelecidos do repositório

## O que vem a seguir

Quando esta implementação estiver concluída e todos os artefatos de prova criados, oriente o usuário a executar `/SDD-4-validate-spec-implementation` para verificar que a implementação atende a todos os requisitos da spec. Isso mantém a progressão do fluxo: ideia → spec → tarefas → implementação → validação.
