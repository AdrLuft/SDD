# PRD — Product Requirements Document

> **Operacional — humanos e IA:** Este arquivo fixa **produto, escopo e intenção de UX**. Limites de **implementação no repositório** (comandos, pastas, stack, o que não existe ainda) estão em [`TECH-SPEC-IA.md`](./TECH-SPEC-IA.md). Para **ordem do fluxo** SDD-1 → SDD-4 e o papel de cada arquivo da pasta, use [`guia-fase-1.md`](./guia-fase-1.md) e [`guia-fase-2.md`](./guia-fase-2.md). Em todo prompt SDD-1…SDD-4, **anexe PRD + TECH-SPEC-IA** junto da descrição da feature.

### Nova feature: o que preencher neste PRD

| Situação | O que fazer |
| --- | --- |
| **Mínimo** | Preencher seções **1–4** (contexto, objetivos, público, escopo funcional). |
| **Recomendado** | Incluir **5** (experiência/conteúdo) e **6** (fora de escopo) quando mudarem o que pode ser codado ou priorizado. |
| **Cadeia SDD** | Usar **§10** para alinhar o que o SDD-1 deve extrair e o checklist antes de codar. |

### Mapa: onde definir cada tipo de regra

| O que você quer definir | Arquivo | Seção ou nota |
| --- | --- | --- |
| Fluxo SDD e ordem dos prompts | `guia-fase-1.md` e `guia-fase-2.md` | Documento inteiro |
| Spec detalhada da feature (artefato) | Saída do SDD-1 | Pastas sob `docs/specs/` (convênção em TECH-SPEC-IA §2) |
| Objetivos, sucesso, métricas | `PRD.md` | §2 |
| Escopo funcional e requisitos | `PRD.md` | §4 |
| Critérios de aceite **de produto** (comportamento / negócio) | `PRD.md` | §2, §4 e §10.2 |
| Definition of Done **técnica** (analyze, testes, provas, deps) | `TECH-SPEC-IA.md` | §10.3 |
| Experiência, conteúdo, UX | `PRD.md` | §5 |
| O que **não** implementar (scope guardrail) | `PRD.md` | §6 |
| Integrações, dependências de negócio, compliance | `PRD.md` | §7 |
| Riscos e decisões em aberto (produto) | `PRD.md` | §8 |
| Como a IA deve **ler** e **priorizar** estes docs | `TECH-SPEC-IA.md` | §1 |
| Arquitetura permitida, limites do repo | `TECH-SPEC-IA.md` | §4 (incl. §4.4 ADR) |
| Backend, APIs, BaaS, auth server | `TECH-SPEC-IA.md` | §7 |
| Toolchain, comandos canônicos | `TECH-SPEC-IA.md` | §3 |
| Variáveis de ambiente | `TECH-SPEC-IA.md` | §3.4 |
| Segurança obrigatória (dados, segredos, auth) | `TECH-SPEC-IA.md` | §9 |
| Padrões de código e estilo | `TECH-SPEC-IA.md` | §5 |
| Pacotes proibidos ou obrigatórios, erros recorrentes | `TECH-SPEC-IA.md` | §13 |
| Git, CI/CD, release | `TECH-SPEC-IA.md` | §11 |

**Regra geral:** guardrail de **produto ou escopo** → `PRD.md`. Guardrail **técnico ou do repositório** → `TECH-SPEC-IA.md`.

---
# PRD — Product Requirements Document

> **Operacional — humanos e IA:** Este arquivo fixa **produto, escopo e intenção de UX**. Limites de **implementação no repositório** (comandos, pastas, stack, o que não existe ainda) estão em [`TECH-SPEC-IA.md`](./TECH-SPEC-IA.md). Para **ordem do fluxo** SDD-1 → SDD-4 e o papel de cada arquivo da pasta, use [`guia-fase-1.md`](./guia-fase-1.md) e [`guia-fase-2.md`](./guia-fase-2.md). Em todo prompt SDD-1…SDD-4, **anexe PRD + TECH-SPEC-IA** junto da descrição da feature.

---

## Metadados do documento

| Campo | Valor |
| --- | --- |
| **Produto / sistema** | ERP genérico (primeiro caso: academia) |
| **Versão do PRD** | 0.4 |
| **Status** | Em desenvolvimento. **Entrega em foco:** apenas auth (cadastro, login, recuperação de conta). |
| **Data** | 06/04/2026 |
| **Autor(es)** | Adriano |
| **Revisores / aprovadores** | N/A |
| **Prazo desejado (se houver)** | N/A |
| **Links** | `TECH-SPEC-IA.md`, `guia-fase-1.md`, `guia-fase-2.md` |

---

## 1. Contexto e problema

### 1.1 Por que isso existe agora?

- **Escopo deste PRD (agora):** implementar **somente** cadastro, login, recuperação de conta, sessão e logout — nada de módulos de negócio na mesma entrega.
- **Contexto de negócio / produto:** O produto começa atendendo uma **academia**, mas a visão é um **ERP genérico** reutilizável. Autenticação e identidade são base para qualquer módulo (alunos, financeiro, estoque, etc.); esses módulos vêm **depois**.
- **Problema atual:** Sem fluxo de **cadastro/login/recuperação** confiável, não dá para atribuir ações a um usuário, proteger dados nem evoluir para papéis/permissões de ERP.
- **Quem sofre o problema hoje?** Dono e equipe da academia (operam sem sistema unificado); no futuro, outros negócios que usarão o mesmo núcleo.

### 1.2 Declaração do problema (uma frase)

> Precisamos de **cadastro, login e recuperação de conta** confiáveis para que cada pessoa tenha acesso próprio ao sistema; sem isso, não há base segura para evoluir o restante do produto (academia / ERP).

### 1.3 Premissas explícitas (o que estamos assumindo como verdade)

1. O primeiro cliente interno é uma **academia** com equipe reduzida e dono envolvido; não há exigência, neste PRD, de multi-tenant B2B completo no primeiro corte (pode evoluir depois).
2. Usuários terão **acesso à internet** no uso principal do sistema; offline é desejável no futuro, não premissa do MVP de identidade.
3. **E-mail** (ou identificador acordado) é aceitável como chave principal de conta para cadastro, login e recuperação.
4. Esta entrega **não** inclui módulos de negócio (alunos, financeiro, treinos) nem **RBAC** avançado; apenas identidade e sessão, deixando o modelo de dados pronto para evoluir depois sem reescrever auth do zero.

### 1.4 O que já sabemos vs. o que é hipótese

| Item | Sabido (evidência) | Hipótese (a validar) |
| --- | --- | --- |
| Dor sem sistema unificado | Operação atual depende de planilhas/WhatsApp (contexto de negócio informado) | "Equipe adotará o sistema se o fluxo diário (check-in, cobrança) estiver no mesmo lugar" |
| Prioridade de identidade | Sem login/cadastro/recuperação não há base para o restante do ERP | Próximas features após auth ficam **fora** deste PRD |
| Canal de uso | A definir no TECH-SPEC-IA (web vs. mobile) | "Maioria do uso será em mobile no balcão" vs. "back-office só desktop" |

---

## 2. Objetivos e sucesso

### 2.1 Objetivo principal (outcome)

Entregar um **fluxo completo de autenticação**: criar conta, entrar com credenciais e recuperar acesso quando necessário, com **sessão** estável e **logout** claro. O resultado é que qualquer tela ou API futura possa assumir "há um usuário identificado" sem gambiarras.

### 2.2 Objetivos específicos (mensuráveis quando possível)

1. **Taxa de conclusão** dos três fluxos P0 (cadastro, login, recuperação) — medir via eventos de produto ou funil; meta após baseline.
2. **Menos dependência de suporte** para "esqueci a senha" — comparar antes/depois quando houver canal de atendimento ou piloto.
3. **Zero regressão crítica** nos fluxos de auth após deploy (login/cadastro/recuperação testáveis de ponta a ponta).

### 2.3 Métricas de sucesso (north star + operacionais)

| Métrica | Definição | Baseline | Meta | Prazo de leitura |
| --- | --- | --- | --- | --- |
| North star (esta entrega) | Usuários conseguem **cadastrar, logar e resetar senha** sem intervenção manual na jornada feliz | N/A | 100% dos fluxos P0 implementados e validados (SDD-4) | Ao fechar a feature |
| Conclusão cadastro | Do início do cadastro até "conta utilizável e sessão ativa" | A definir | ≥ 80% dos inícios | Mensal no piloto |
| Conclusão recuperação | Pedido de reset → nova senha definida em ≤ 15 min | A definir | ≥ 85% | Mensal no piloto |
| Disponibilidade auth | Uptime ou taxa de erro 5xx nos endpoints de auth | A definir | Alinhar ao TECH-SPEC-IA | Contínuo |

### 2.4 Anti-métricas (o que não pode piorar)

- **Latência p95** do login e do carregamento pós-login não pode degradar perceptivelmente vs. baseline após mudanças.
- **Taxa de falha de login** por bug (não por senha errada) deve permanecer próxima de zero; picos disparam revisão imediata.
- **Custo por usuário ativo** (e-mail transacional, etc.) não deve crescer de forma desproporcional ao número de contas reais.
- **Carga em suporte manual** (reset manual de senha) não deve ser o caminho padrão após o fluxo de recuperação estar no ar.

---

## 3. Público, personas e permissões

### 3.1 Usuários / personas

| Persona | Necessidade (nesta entrega) | Familiaridade técnica | Frequência de uso |
| --- | --- | --- | --- |
| Dono / gestor | Criar sua conta, entrar, recuperar senha | Média | Conforme piloto |
| Equipe (recepção / instrutores) | Conta própria para **login** no app | Baixa a média | Diária (após haver app pós-login) |
| Alunos | Acessar informações pessoais e realizar check-ins | Baixa | Diária (após implementação de módulos futuros) |

*Personas e necessidades de **financeiro** e **contador** ficam fora até as features respectivas.*

### 3.2 Casos fora do público-alvo (explícito)

- **Qualquer fluxo de negócio** após o login (alunos, mensalidades, treinos, estoque, etc.) — **fora desta entrega.**
- **RBAC / admin vs. operador** com regras por tela — **fora**; pode existir apenas usuário "genérico" autenticado até uma feature de permissões.
- **Marketplace / multi-tenant B2B complexo** — fora; decisão de "uma organização vs. várias" pode ficar simplificada ou adiada na spec.

### 3.3 Papéis, permissões e dados sensíveis

- **Quem pode ver / fazer o quê?** Apenas o necessário para **auth**: qualquer usuário autenticado acessa o que o produto expuser **após** o login nesta fase. **Sem** matriz de permissões neste PRD.
- **Dados pessoais / LGPD:** Cadastro implica **e-mail**, nome, idade e sexo. Política de privacidade e termos antes de escalar além de piloto.
- **Auditoria:** Desejável registrar **criação de conta**, **login bem-sucedido** e **troca de senha / reset** em nível mínimo para suporte e segurança.

---

## 4. Escopo funcional

### 4.1 Visão resumida da solução (nível produto, não implementação)

O usuário **cadastra** uma conta com identificador e senha (e demais campos mínimos definidos na spec), **entra** no sistema quando já tem conta e, se esquecer a senha, **recupera o acesso** por um fluxo seguro (ex.: e-mail com link). Depois de autenticado, permanece **logado** até **sair** ou expirar a sessão. Nada além disso é obrigatório nesta entrega.

### 4.2 Requisitos funcionais de produto (priorizados)

Use IDs estáveis para rastrear até spec, tarefas e validação. **Escopo deste PRD = somente as linhas abaixo.**

| ID | Prioridade (P0/P1/P2) | Requisito (testável) | Notas |
| --- | --- | --- | --- |
| PRD-RF-001 | P0 | O sistema deve permitir **cadastro** de usuário com dados mínimos acordados e confirmação de identidade se a política de produto exigir. | Campos obrigatórios: nome, e-mail, idade, sexo e senha. Evitar conta duplicada pelo mesmo identificador. |
| PRD-RF-002 | P0 | O sistema deve permitir **login** com credenciais válidas e **recusar** com mensagem segura quando inválidas (sem revelar se o e-mail existe). | Rate limiting é detalhe técnico. |
| PRD-RF-003 | P0 | O sistema deve oferecer **recuperação de conta / senha** por canal acordado (ex.: link por e-mail) com expiração e invalidação após uso. | Copy e prazos na spec. |
| PRD-RF-004 | P0 | O sistema deve manter **sessão** de forma que o usuário permaneça autenticado conforme política de segurança até logout ou expiração. | "Lembrar-me" opcional — decidir na spec. |
| PRD-RF-005 | P0 | O sistema deve permitir **logout** explícito que encerre a sessão no cliente e invalide o acesso no servidor conforme modelo adotado. | — |

*Roadmap (não faz parte desta entrega): papéis/permissões, alunos, financeiro, treinos — novos PRDs ou incrementos de PRD quando forem priorizados.*

**Critérios de qualidade para cada linha:**
- Verbo claro (deve / não deve)
- Condição e resultado observável
- Sem jargão de implementação ("usar Redis", "criar microserviço") — isso vai para TECH-SPEC-IA ou spec técnica

### 4.3 Fluxos principais (cenários ponta a ponta)

1. **Fluxo: Cadastro**
   - **Feliz:** Gatilho "criar conta" → preenche dados (nome, e-mail, idade, sexo, senha) → (opcional) confirma e-mail → sessão ativa ou tela de login com mensagem de sucesso → área autenticada mínima.
   - **Erro / vazio / sem permissão:** E-mail já usado; validação de senha fraca; token de confirmação expirado; mensagens claras e ação seguinte (ir para login, reenviar e-mail).

2. **Fluxo: Login**
   - **Feliz:** Credenciais corretas → sessão ativa → redirecionamento para destino padrão ou deep link.
   - **Alternativos:** "Esqueci minha senha"; troca de senha obrigatória se política exigir.
   - **Erro / vazio / sem permissão:** Credenciais inválidas; conta desativada; mensagem genérica + opção de recuperação; bloqueio temporário após N tentativas.

3. **Fluxo: Recuperação de senha**
   - **Feliz:** Solicita reset → recebe link por e-mail → define nova senha → login com nova senha.
   - **Alternativos:** Reenvio de e-mail; suporte manual só como último recurso documentado.
   - **Erro / vazio / sem permissão:** Link expirado ou já usado; e-mail inexistente (mesma resposta que sucesso para não enumerar contas).

### 4.4 Estados e casos extremos obrigatórios

- **Vazio:** Nenhuma conta ainda — primeiro cadastro deve resultar em usuário utilizável.
- **Grande volume:** Limites e performance dos endpoints de auth na spec.
- **Concorrência:** Mesmo usuário com duas sessões ou troca de senha durante sessão ativa — comportamento seguro (invalidar tokens antigos, etc.) na spec.
- **Offline / indisponibilidade de serviço:** Mensagem clara; sem "falso positivo" de login; retry seguro na recuperação de senha.
- **Internacionalização:** Português (Brasil) nesta entrega; outros idiomas fora do escopo.
- **Acessibilidade:** Teclado e foco nos três fluxos; alvo WCAG na spec de UI quando aplicável.

---

## 5. Experiência e conteúdo

### 5.1 Princípios de UX (desta entrega)

- **Mínimo de passos** nos fluxos P0 (cadastro, login, recuperação).
- **Erro recuperável:** sempre indicar próxima ação ("tentar de novo", "ir para login", "reenviar e-mail").
- **Linguagem plain** para staff não técnico; evitar jargão de segurança excessivo na UI.
- **Consistência:** mesmos padrões de formulário, feedback e estados de carregamento em todas as telas de identidade.

### 5.2 Referências de design (feature Auth)

- **Estilo:** Material 3, paleta de cores neutra com um acento primário.
- **Formulários:** validação em tempo real nos campos.
- **Feedback:** snackbar ou indicador inline para erros; estado de loading visível no botão de submit durante requisições.
- **Navegação:** botão primário único por tela, sem modais desnecessários.
- **Figma / protótipo:** Não há — definir no repo alvo quando existir.
- **Copy / tom de voz:** Profissional, direto, empático em erros; sem culpar o usuário.

### 5.3 Notificações, e-mails e mensagens de erro

- **E-mail de confirmação / reset:** usuário que cadastrou ou solicitou recuperação; conteúdo mínimo: quem enviou, expiração do link, ignorar se não foi ele.
- **Erros de login:** mensagem genérica + link "esqueci minha senha".
- **Opt-in marketing:** fora do escopo dos e-mails transacionais de auth.

---

## 6. Fora de escopo (não objetivos)

1. **Módulos de academia / ERP** (cadastro de alunos, planos de treino, mensalidades, estoque, etc.) — **Motivo:** esta entrega é só **auth**.
2. **RBAC, convites por admin, gestão de equipe** — **Motivo:** amarrar permissões e convites exige decisões de produto que não são necessárias para cadastro/login/recuperação básicos.
3. **SSO corporativo (SAML/OIDC)** — **Motivo:** complexidade desproporcional para o primeiro corte.
4. **App mobile nativo só para auth** — **Motivo:** web responsiva ou PWA costuma bastar; nativo é decisão futura.
5. **2FA / biometria obrigatórios** — **Motivo:** opcional em fase posterior salvo exigência regulatória explícita.
6. **Login social (Google, Apple)** — **Motivo:** será feature separada se priorizado.

---

## 7. Integrações, dependências e compliance

### 7.1 Dependências externas / internas

| Dependência | Tipo | Dono | Risco se atrasar |
| --- | --- | --- | --- |
| Provedor de e-mail transacional (SMTP/API) | Bloqueante para recuperação/confirmação | Produto / eng | Usuários presos sem reset automático |
| Domínio e DNS (SPF/DKIM) para deliverability | Bloqueante para confiança em e-mail | Operações | E-mails na caixa de spam ou bloqueados |
| Repositório e TECH-SPEC-IA preenchidos | Interna | Time | Specs e tarefas com comandos/paths inventados |
| Hospedagem / banco conforme stack | Bloqueante | Eng | Nenhum deploy testável |

### 7.2 Requisitos legais / políticas

- **LGPD:** tratamento de dados pessoais com finalidade informada; base legal para cadastro operacional.
- **Termos de uso e política de privacidade:** publicação antes de escalar usuários externos além de piloto controlado.
- **Senha e dados sensíveis:** política mínima de complexidade e armazenamento conforme boas práticas.

### 7.3 Analytics e experimentação

- **Eventos a disparar (sugestão):** `signup_started`, `signup_completed`, `login_success`, `login_failure`, `password_reset_requested`, `password_reset_completed`, `logout`.
- **Feature flag:** opcional para **recuperação de senha** ou política de confirmação de e-mail até estabilizar.

---

## 8. Riscos, decisões e questões em aberto

### 8.1 Riscos

| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| Scope creep para módulos de negócio | M | A | PRD e seção 6 fixam "só auth"; qualquer extra exige novo acordo |
| E-mails de reset não entregues | M | A | Provedor confiável, SPF/DKIM, monitorar bounce; runbook |
| Confirmação de e-mail mal definida trava UX | M | M | Decidir cedo na spec (seção 8.3); default explícito |
| Sessão / tokens mal especificados geram brecha | B | A | Revisão de segurança na spec técnica e testes |
| Dependência do Supabase para autenticação | M | A | Garantir fallback ou plano de contingência caso o serviço fique indisponível |

### 8.2 Decisões já tomadas (evita reabrir na spec)

1. **Decisão:** Primeiro caso de uso é **academia**, com visão de ERP reutilizável. **Racional:** domínio conhecido para validar núcleo antes de generalizar.
2. **Decisão:** A **única feature** priorizada agora é **auth** (cadastro, login, recuperação). **Racional:** base única antes de construir o restante do ERP.

### 8.3 Questões em aberto (bloqueiam detalhamento?)

| # | Questão | Dono | Prazo |
| --- | --- | --- | --- |
| 1 | Confirmação de e-mail é obrigatória antes de usar o sistema? | Produto | Antes de fechar SDD-1 |
| 2 | Cadastro é **aberto** (qualquer um cria conta) ou **fechado** (só convite / admin)? | Produto | Antes de fechar SDD-1 |
| 3 | Após o login, qual é a **tela mínima** aceitável (home vazia, "em breve", redirect)? | Produto | Antes de fechar SDD-1 |

---

## 9. Lançamento e operação

### 9.1 Estratégia de release

**Incremental:** liberar **apenas** fluxos de auth em piloto; próximas features em releases separados. **Kill switch** opcional em recuperação de senha se houver instabilidade.

### 9.2 Suporte e documentação para o usuário

- FAQ curta: criar conta, login, "esqueci minha senha".
- Runbook interno: checar entrega de e-mail, spam, reenvio de link de reset.
- Treinamento mínimo para quem for testar o piloto.

### 9.3 Plano de rollback

- Reverter deploy da aplicação para versão estável anterior.
- Desligar feature flags que introduziram regressão.
- Manter backup de banco conforme procedimento do TECH-SPEC-IA antes de migrações arriscadas.

---

## 10. Alinhamento com o fluxo SDD (obrigatório se você usa SDD-1…4)

### 10.1 O que o SDD-1 deve extrair deste PRD

Ao gerar `./docs/specs/[NN]-spec-[nome]/`, a spec **deve** refletir:

- Objetivos e **histórias de usuário** derivados das personas e fluxos
- **Unidades demonstráveis** alinhadas aos fluxos P0 (fatias verticais, não "camadas órfãs")
- **Requisitos funcionais** da spec mapeáveis a estes IDs de produto (`PRD-RF-xxx`) ou explícita divergência justificada
- **Artefatos de prova** pensados já aqui: _o que será mostrado_ (URL, CLI, teste, screenshot) para cada fatia P0
- **Não objetivos** coerentes com a seção 6 deste PRD

### 10.2 Critérios de aceite em nível PRD (antes de codar)

- [ ] Escopo **só auth** e prioridades P0 (PRD-RF-001 a PRD-RF-005) acordados com stakeholders
- [ ] Métricas e anti-métricas definidas ou explicitamente "N/A" com motivo
- [ ] Fluxos feliz/erro/vazio cobertos
- [ ] Fora de escopo revisado para impedir scope creep
- [ ] Questões bloqueantes resolvidas ou registradas como risco aceito

---

## 11. Histórico de alterações

| Versão | Data | Autor | Mudança |
| --- | --- | --- | --- |
| 0.1 | 04/04/2026 | Adriano | PRD inicial preenchido: ERP/academia, P0 identidade, RFs e fluxos |
| 0.2 | 04/04/2026 | Adriano | Escopo da entrega atual: somente auth (cadastro, login, recuperação) |
| 0.3 | 05/04/2026 | Adriano | Cabeçalho operacional visível: mapa PRD/TECH/GUIA, separação aceite produto vs DoD técnica |
| 0.4 | 06/04/2026 | Adriano | Inclusão da feature de autenticação: novas personas, UX e riscos; remoção dos RFs duplicados 006–008; campos obrigatórios movidos para notas do RF-001; seções de guia humano movidas para PRD-GUIA-HUMANO.md |---

### Checklist crítico (autoavaliação honesta)

- [ ] Um leitor **não técnico** entende **o quê** e **por quê**
- [ ] Um engenheiro consegue estimar **incerteza** (áreas ainda abertas estão listadas)
- [ ] Não há mistura excessiva de solução técnica (isso pertence ao **TECH-SPEC-IA.md** e à spec)
- [ ] Cada P0 tem caminho de verificação pensado (mesmo que ainda sem implementação)
- [ ] Segurança e dados sensíveis foram considerados **antes** da spec
