

---

# GUIA — FASE 2: Executar o fluxo SDD

## Instrução para a IA

Os arquivos `PRD.md` e `TECH-SPEC-IA.md` já foram atualizados e aprovados na Fase 1. Execute agora o fluxo SDD estritamente na ordem abaixo. Em cada etapa, os dois arquivos devem estar anexados à conversa.

### ⚠️ Checklist pré-voo — confirme ANTES de iniciar qualquer etapa

- [ ] `PRD.md` atualizado e aprovado na Fase 1 está anexado nesta conversa
- [ ] `TECH-SPEC-IA.md` atualizado (ou confirmado sem alterações) está anexado nesta conversa
- [ ] A Fase 1 foi concluída com confirmação explícita do usuário

**Não inicie nenhuma etapa SDD sem esses três itens confirmados.**

---

## ETAPA 1 → SDD-1-generate-spec.md

**O que acontece nesta etapa:**

1. Abra (ou carregue) o arquivo `SDD-1-generate-spec.md` junto com `PRD.md` e `TECH-SPEC-IA.md`
2. Descreva a feature para a IA usando os dados preenchidos na Fase 1
3. A IA irá: avaliar escopo, criar diretório `./docs/specs/[NN]-spec-[nome]/` e gerar um arquivo de perguntas `[NN]-questions-1-[nome].md`
4. Responda as perguntas diretamente no arquivo gerado e salve
5. Pode haver múltiplas rodadas de perguntas (questions-1, questions-2...) — **perguntas sobre UI/design são obrigatórias** — responda todas antes de avançar
6. A IA gera a spec. Você aprova: "Esta spec reflete seus requisitos? Os limites de escopo e as **considerações de design** estão corretos?"
7. **Verifique que a seção "Considerações de design" da spec está completa** (cores, tipografia, layout, componentes, estados, responsividade)
8. **Só avance para a Etapa 2 após aprovação explícita da spec**

---

## ETAPA 2 → SDD-2-generate-task-list-from-spec.md

**O que acontece nesta etapa:**

1. Abra `SDD-2-generate-task-list-from-spec.md` apontando para a spec aprovada na Etapa 1
2. A IA gera as tarefas pai primeiro e apresenta para aprovação
3. Confirme as tarefas pai antes de a IA gerar as subtarefas
4. **Só avance para a Etapa 3 após a lista de tarefas estar completa e aprovada**

---

## ETAPA 3 → SDD-3-manage-tasks.md

**O que acontece nesta etapa:**

1. Abra `SDD-3-manage-tasks.md` com a lista de tarefas aprovada
2. A IA executa as tarefas na ordem definida, roda testes e lint após cada tarefa, e grava artefatos de prova
3. **Para tarefas de UI:** a IA DEVE executar o quality gate visual (overflow, responsividade, estados) antes de marcar como concluída
4. Acompanhe a execução — não pule validações intermediárias
5. **Só avance para a Etapa 4 após todas as tarefas estarem concluídas e com provas gravadas**

---

## ETAPA 4 → SDD-4-validate-spec-implementation.md

**O que acontece nesta etapa:**

1. Abra `SDD-4-validate-spec-implementation.md`
2. A IA valida a implementação contra a spec aprovada na Etapa 1 e gera relatório de validação
3. Se reprovar (total ou parcialmente):
   - Corrija os itens reprovados conforme o relatório
   - Revalide executando a Etapa 4 novamente
   - Se a correção exigir mudança de escopo ou arquitetura → volte à Fase 1 antes de continuar
4. **A feature só está concluída quando o SDD-4 emitir aprovação**

---

## 🚫 Guardrails da Fase 2 — a IA NUNCA deve

- Pular etapas ou executar qualquer SDD fora da ordem 1 → 2 → 3 → 4
- Implementar código antes de a spec estar aprovada (Etapa 1 concluída)
- Adicionar pacotes ao `pubspec.yaml` sem justificativa registrada na spec aprovada
- Assumir Node, npm, Laravel, Firebase ou qualquer backend/serviço não definido explicitamente na spec
- Commitar tokens, senhas, chaves de API ou PII real nos artefatos de prova
- Expandir o escopo além do que está na seção §4 do PRD atualizado na Fase 1
- Definir métricas numéricas de sucesso — isso é responsabilidade do humano
- Criar arquivos de implementação em `bin/` — esta pasta é apenas para scripts CLI auxiliares
- Criar telas soltas em `lib/screens/` — toda feature deve seguir Clean Architecture em `lib/features/<nome>/`
- Criar telas sem scroll em layouts com conteúdo dinâmico — usar `SingleChildScrollView` ou `ListView`
- Criar telas sem implementar TODOS os estados visuais (loading, erro, sucesso, vazio)
- Ignorar a seção "Considerações de design" da spec ao implementar UI — cores, tipografia, espaçamento e componentes DEVEM seguir o especificado
- Marcar tarefa de UI como concluída sem executar o quality gate visual (overflow, responsividade, estados, acessibilidade)