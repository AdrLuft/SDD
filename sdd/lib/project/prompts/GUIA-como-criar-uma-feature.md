# Guia rápido: criar uma feature com SDD

## O fluxo (em uma linha)

**Ideia** → você mantém **PRD** + **TECH-SPEC-IA** no projeto → **SDD-1** (spec) → **SDD-2** (tarefas) → **SDD-3** (código) → **SDD-4** (validação).

Os prompts **SDD-1 … SDD-4** são instruções para a IA. **PRD** e **TECH-SPEC-IA** são **contexto fixo do projeto**: você preenche bem uma vez e **não precisa reescrever a cada feature** — na próxima feature você descreve a ideia no **SDD-1** e **anexa os mesmos** arquivos. Só volte a editar PRD/TECH quando algo do **produto** ou do **repo** mudar (ver abaixo).

---

## Pasta `sdd/lib/project/prompts` — cada arquivo

Tudo que está aqui são **modelos e prompts** do fluxo SDD. No **seu** app, só precisa **copiar** (e preencher) **PRD** e **TECH-SPEC-IA**; o resto você **abre ou cola** como instrução para a IA quando for o passo certo.

| Arquivo | Para que serve |
| --- | --- |
| `GUIA-como-criar-uma-feature.md` | **Este guia:** visão geral do fluxo e mapa da pasta (leitura humana; não é prompt obrigatório para a IA). |
| `PRD.md` | **Template de produto:** problema, usuários, escopo, prioridades, métricas, fora de escopo. Preencha no repo alvo e anexe no SDD-1 (e quando fizer sentido). |
| `TECH-SPEC-IA.md` | **Template técnico do repo:** stack, comandos reais de test/lint/build, estrutura de pastas, convenções e anti-padrões. Preencha no repo alvo e anexe com o PRD. |
| `SDD-1-generate-spec.md` | **Prompt:** a IA gera a spec detalhada (e costuma criar `docs/specs/...`), fazendo perguntas antes de fechar. |
| `SDD-2-generate-task-list-from-spec.md` | **Prompt:** a IA gera a lista de tarefas a partir da spec aprovada (pais primeiro, depois subtarefas conforme o texto do prompt). |
| `SDD-3-manage-tasks.md` | **Prompt:** a IA executa tarefas, atualiza checkboxes, roda testes/lint, grava provas e commits no ritmo definido no prompt. |
| `SDD-4-validate-spec-implementation.md` | **Prompt:** a IA compara implementação + provas com a spec e produz relatório de validação (aprovar/reprovar e o que corrigir). |

Coloque **PRD** e **TECH-SPEC-IA** preenchidos na **raiz ou `docs/`** do repo onde você codifica; nas mensagens use `@PRD.md` e `@TECH-SPEC-IA.md`.

---

## Uma vez no projeto

1. Copie e preencha **PRD** e **TECH-SPEC-IA** (troque todos os placeholders por dados reais).
2. No **TECH-SPEC-IA**, use **comandos exatos** do seu CI (ex.: `pnpm test`), não frases genéricas.
3. A IA costuma criar algo como `docs/specs/01-nome-feature/` com spec, tarefas, `proofs/` e relatório de validação — você não precisa criar a mão.

### Quando ajustar PRD e TECH-SPEC-IA de novo

| Situação | O que fazer |
| --- | --- |
| **Nova feature** | Em geral **nada**: anexe o PRD e o TECH-SPEC-IA atuais e siga SDD-1 → 4. O que muda é a **descrição da feature** e os arquivos novos em `docs/specs/...`. |
| **PRD** | Edite se mudar visão de produto, personas, prioridades globais, métricas ou o que é “fora de escopo” para o app inteiro. |
| **TECH-SPEC-IA** | Edite se mudarem comandos de CI, pastas, stack, convenções — ou se a IA **repetir** o mesmo erro (registre a regra no doc). |

---

## Passos (ordem fixa)

1. **Clarear** — problema, para quem, sucesso; P0 mínimo no PRD. Ideia gigante → dividir em várias features/specs.
2. **SDD-1** — descreva a feature e anexe PRD + TECH-SPEC-IA. Responda às perguntas da IA. Revise a spec antes de seguir.
3. **SDD-2** — aponte o arquivo da spec; confirme pais antes de subtarefas, se o prompt pedir.
4. **SDD-3** — implementação, checkboxes, testes/lint, provas, commits (como o prompt descreve).
5. **SDD-4** — validação; se reprovar, corrija conforme o relatório.

Ordem dos prompts: **SDD-1 → SDD-2 → SDD-3 → SDD-4**.

---

## Checklist antes da IA (SDD-1)

- [ ] Ideia em um parágrafo  
- [ ] PRD com P0 e fora de escopo razoáveis  
- [ ] TECH-SPEC-IA com comandos reais  
- [ ] Vou anexar PRD + TECH-SPEC-IA  

---

## Se travar

| Problema | Ação |
| --- | --- |
| Spec enorme | Reduzir escopo no PRD ou pedir para dividir em 2+ specs |
| Tarefas vagas | Enriquecer a spec e rodar SDD-2 de novo |
| IA inventa stack/pasta | Reforçar TECH-SPEC-IA (“só o que existe no repo”) |
| SDD-4 reprova | Seguir o relatório; corrigir código ou completar `proofs/` |

Quando um ciclo mostrar lacuna no contexto técnico, **atualize o TECH-SPEC-IA**; não é obrigatório mexer nele em toda feature, só quando fizer sentido.
