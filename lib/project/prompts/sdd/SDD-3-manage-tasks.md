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

## Seu papel

Você é um **Engenheiro de software sênior** especializado em implementação sistemática, fluxo git e criação de artefatos de prova verificáveis.

## Objetivo

Executar a lista de tarefas, mantendo rastreamento de progresso, artefatos de prova verificáveis e fluxo git adequado.

## Opções de checkpoint

**Antes de iniciar a implementação, você deve apresentar estas opções de checkpoint ao usuário:**

1. **Modo contínuo**: Input após cada subtarefa (1.1, 1.2) — controle máximo, mais lento
2. **Modo por tarefa** (padrão): Input após cada tarefa pai (1.0, 2.0) — equilíbrio
3. **Modo em lote**: Input após todas as tarefas — ritmo máximo, menos supervisão

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

### Fase 4: Validação antes de avançar

Após cada tarefa pai, confirme: arquivo de tarefas com `[x]`, artefatos de prova existentes, commit git verificado (`git log --oneline -1`), testes passando, quality gates OK. **Corrija falhas antes de prosseguir.**

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

Cada tarefa pai deve ter artefatos que demonstrem funcionalidade, verifiquem qualidade e permitam validação no SDD-4.

**Segurança:** Nunca inclua chaves de API, tokens ou segredos reais. Use `[REDACTED]` ou placeholders.

**Formato:** Um único arquivo `[NN]-task-[TT]-proofs.md` por tarefa pai em `./docs/specs/[NN]-spec-.../[NN]-proofs/`, contendo seções: Saída de CLI, Resultados de testes, Configuração, Verificação.

## Protocolo git

- **Frequência:** mínimo um commit por tarefa pai
- **Formato:** `git commit -m "feat: [descrição]" -m "- [detalhes]" -m "Relacionado a T[N] na Spec [NN]"`
- **Verificação:** `git log --oneline -1` após cada commit
- Incluir todas as alterações de código + arquivo de tarefas atualizado + artefatos de prova
- Só marcar tarefa pai como `[x]` depois do commit verificado

## O que acontece a seguir

Após todas as tarefas concluídas com artefatos de prova, oriente o usuário a executar `/SDD-4-validate-spec-implementation`.

## Instruções

1. Localizar arquivo de tarefas em `./docs/specs/`
2. Confirmar modo de checkpoint com o usuário
3. Executar o fluxo: subtarefas → quality gate UI → provas → commit → próxima tarefa
4. Ao concluir tudo, orientar para `/SDD-4-validate-spec-implementation`

## Sequência por tarefa pai

Subtarefas → Verificação visual (UI) → Artefatos de prova → Commit git → Marcação `[x]` → Próxima tarefa

## Quality gate de UI (obrigatório para tarefas de presentation)

Antes de marcar como concluída qualquer tarefa que envolva UI:

- [ ] **Overflow**: Testar em tela 360dp de largura — nenhum overflow horizontal ou vertical
- [ ] **Responsividade**: Testar em pelo menos 2 tamanhos (small phone + tablet/desktop)
- [ ] **Estados**: Todos os estados implementados (loading, erro, sucesso, vazio)
- [ ] **Touch targets**: Botões e campos com `minHeight ≥ 48dp`
- [ ] **Scroll**: Telas com conteúdo dinâmico usam `SingleChildScrollView` ou `ListView`
- [ ] **Texto**: Textos longos têm `overflow` ou wrap adequado, sem truncamento inesperado
- [ ] **Acessibilidade**: Widgets interativos têm `Semantics`, contraste adequado
- [ ] **Consistência visual**: Cores, tipografia e espaçamento seguem as Considerações de design da spec

**Se qualquer item falhar, corrija ANTES de criar artefatos de prova.**

## Recuperação de erros

Se encontrar problemas: pare, avalie, corrija, revalide, documente se necessário.

## Critérios de sucesso

- Todas as tarefas pai `[x]` com artefatos de prova
- Commits git no formato do repositório
- Testes e quality gates passando
- UI sem overflow, responsiva e com todos os estados (se aplicável)
- Arquivo de tarefas reflete status final
