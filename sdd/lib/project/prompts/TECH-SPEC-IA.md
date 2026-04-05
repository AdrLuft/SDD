# TECH-SPEC-IA — Especificação técnica para IA

> **Leitura obrigatória (humanos e IA):** Este arquivo vive em `sdd/lib/project/prompts/` com os prompts SDD. **Anexe-o sempre com** [`PRD.md`](./PRD.md) ao rodar SDD-1…SDD-4. **Objetivo:** reduzir alucinação amarrando **estrutura real do repo**, **comandos Flutter/Dart** e **limites** (ex.: backend de auth ainda não definido no código). **Regra crítica:** o que não estiver aqui nem no código **não é lei** — não inventar API, pasta ou pacote. Em outro repositório de app, copie este arquivo (e o PRD) para a raiz do pacote ou `docs/` e ajuste paths e comandos. Fluxo e ordem dos passos: [`GUIA-como-criar-uma-feature.md`](./GUIA-como-criar-uma-feature.md).

### O que costuma mudar quando entra uma feature nova

| Alvo | Seção | Quando atualizar |
| --- | --- | --- |
| ADR (decisão arquitetural) | §4.4 | Nova escolha estável: roteamento, estado, BaaS, cliente HTTP, etc. |
| Backend / contrato | §7 | A feature fixa serviço, endpoints, modelo de token ou provedor (ex.: auth). |
| Variáveis de ambiente | §3.4 | Novas chaves, URLs ou integrações externas. |
| Histórico deste documento | §16 | Sempre que mudar versão ou conteúdo relevante. |

### Baseline do repositório (não “reescrever” a cada feature)

As seções **1**, **4** (exceto acrescentar ADR em §4.4), **5**, **9**, **10**, **11** e **13** descrevem **política e limites** do projeto. Por feature você **cumpre** essas regras; não substitua o arquivo por um doc paralelo.

**Quando mesmo assim editar:** se o time ou a IA **repetirem o mesmo erro**, adicione um bullet em **§13** (anti-padrões) ou refine **§5**. Se a **stack ou a segurança** mudarem, atualize as seções afetadas e **§16**.

### Resumo

Sem nova ADR em §4.4, sem mudança de backend em §7 e sem novas env vars em §3.4 → em geral **nenhuma alteração** neste arquivo; evoluem sobretudo a spec em `docs/specs/` e o código.

---

## Metadados

| Campo | Valor |
| --- | --- |
| **Repositório** | SDD (workspace local `C:/Projetos/SDD`; app em `sdd/`) |
| **Versão do doc** | 0.3 |
| **Última atualização** | 2026-04-05 |
| **Dono técnico** | Adriano |
| **Stack principal** | Flutter (Dart SDK ^3.9.2) — app em `sdd/` |

---

## 1. Como a IA deve usar este documento

1. **Ler primeiro** este arquivo e a seção 2 antes de propor arquitetura ou paths.  
2. **Não introduzir** dependências ou serviços listados como proibidos (seção 13).  
3. **Repetir exatamente** os comandos da seção 3 em planos, CI e artefatos de prova.  
4. Em dúvida entre “elegante” e **consistente com o repo**, priorizar consistência.  
5. **Backend / auth server:** não há implementação neste repositório; a spec da feature de auth deve **definir** API, stack e onde o código backend mora (ou BaaS). Até isso estar na spec, a IA não deve assumir Node, Laravel, Firebase, etc. como verdade.

---

## 2. Mapa do repositório e leitura obrigatória

### 2.1 Diretórios importantes

| Caminho | Conteúdo |
| --- | --- |
| `sdd/` | Raiz do **pacote Flutter** (`pubspec.yaml`, `lib/`, `test/`, `android/`). É aqui que se roda `flutter pub get`, `flutter test`, etc. |
| `sdd/lib/` | Código Dart do app (`main.dart` e futuras features). |
| `sdd/lib/project/prompts/` | Prompts SDD, **PRD.md**, **TECH-SPEC-IA.md** (este arquivo). |
| `sdd/test/` | Testes (`flutter_test`). |
| `sdd/docs/specs/` | **Criar quando necessário:** specs SDD (`[NN]-spec-.../`), tarefas, `proofs/`. Convenção recomendada para este monorepo. |

### 2.2 Documentos que a IA deve consultar

| Arquivo | Propósito |
| --- | --- |
| `sdd/pubspec.yaml` | Dependências e versão do SDK Dart. |
| `sdd/analysis_options.yaml` | Lints (`flutter_lints`). |
| `sdd/lib/project/prompts/PRD.md` | Escopo de produto (ex.: auth apenas). |
| `sdd/lib/main.dart` | Ponto de entrada atual do app. |

**Não existe** no repo (ainda): `README.md` na raiz do workspace, CI YAML, backend, OpenAPI.

---

## 3. Ambiente, toolchain e comandos canônicos

> Todos os comandos abaixo assumem shell com **Flutter SDK** instalado e cwd = `sdd/`.

### 3.1 Versões

| Ferramenta | Versão |
| --- | --- |
| Dart (via `pubspec`) | SDK ^3.9.2 |
| Flutter | Alinhar com o canal estável que suporta esse SDK (ver `flutter --version` na máquina). |
| Gerenciador | `flutter pub` / Pub (não npm). |

### 3.2 Setup local

```bash
cd sdd
flutter pub get
```

### 3.3 Comandos do dia a dia

| Ação | Comando |
| --- | --- |
| Resolver dependências | `flutter pub get` |
| Análise estática (lints + analyzer) | `flutter analyze` |
| Testes | `flutter test` |
| Formatar | `dart format .` |
| Rodar app (debug) | `flutter run` |
| Build APK (exemplo) | `flutter build apk` |

**Estado conhecido:** `test/widget_test.dart` ainda descreve o template do contador; `lib/main.dart` foi simplificado. Até alinhar teste e UI, `flutter test` pode falhar — **corrigir na primeira tarefa** que tocar em testes ou alinhar com a UI real.

### 3.4 Variáveis de ambiente

| Nome | Obrigatória? | Descrição | Exemplo seguro |
| --- | --- | --- | --- |
| *(a definir na spec de auth)* | — | URL base da API, chaves de e-mail transacional, etc. | Documentar em `.env.example` quando existir backend/cliente HTTP |

**Regras:** nunca commitar segredos; quando houver integração, usar `.env.example` + leitura via `--dart-define` ou pacote acordado na spec.

---

## 4. Arquitetura e limites

### 4.1 Estilo arquitetural (o que o repo **é**)

**App Flutter único** em `sdd/`, sem packages internos no momento. Evolução sugerida (não obrigatória até a spec dizer): pastas por feature sob `lib/` (ex.: `lib/auth/`, `lib/core/`).

### 4.2 Camadas e dependências permitidas

```
UI (Widgets) → (futuro) camada de aplicação / estado → (futuro) cliente HTTP / repositórios
```

- **Permitido:** dependências declaradas em `pubspec.yaml`.  
- **Proibido:** copiar código gerado ou segredos para `docs/specs/.../proofs`.

### 4.3 Contratos públicos

- **APIs HTTP:** nenhuma documentada no repo; definir na spec de auth (OpenAPI ou contrato Dart).  
- **Filas / eventos:** N/A.  
- **Banco:** N/A no cliente; persistência servidor conforme backend escolhido na spec.

### 4.4 Decisões fixas (ADR resumidas)

| ID | Decisão | Motivo | Onde está detalhado |
| --- | --- | --- | --- |
| ADR-001 | App Flutter em `sdd/` | Estrutura atual do repositório | Este doc + `pubspec.yaml` |

---

## 5. Padrões de código e estilo

### 5.1 Linguagem e formatação

- **Guia:** [Effective Dart](https://dart.dev/effective-dart) + regras do pacote `flutter_lints` (via `analysis_options.yaml`).  
- **Config:** `sdd/analysis_options.yaml`.  
- **Formatação:** `dart format .` antes de commit quando o time adotar.

### 5.2 Nomenclatura

| Tipo | Convenção | Exemplo |
| --- | --- | --- |
| Arquivos | `snake_case.dart` | `login_page.dart` |
| Classes / enums | `UpperCamelCase` | `LoginPage` |
| Membros / funções | `lowerCamelCase` | `signIn` |

### 5.3 Imports

- Preferir imports relativos ou `package:sdd/...` conforme já usado em `test/widget_test.dart`.

### 5.4 Tratamento de erros

- Usar `Future`/async com try/catch onde fizer sentido; para fluxos de auth, mapear falhas de rede e validação para mensagens de UI (detalhe na spec).

### 5.5 Logging

- Evitar `print` em código de produção quando lint exigir; usar abordagem acordada na spec (`debugPrint`, logger, etc.).

---

## 6. Frontend (Flutter)

- **Framework:** Flutter (Material 3 via `ThemeData` em `main.dart`).  
- **Roteamento:** `MaterialApp` padrão; evoluir para `go_router` ou equivalente **só** se a spec listar o pacote e a migração.  
- **Estado:** `StatefulWidget` hoje; para auth, preferir padrão definido na spec (ex.: `Riverpod`, `Bloc`) — **não adicionar** pacote de estado sem decisão na spec.  
- **Acessibilidade:** Semantics e foco nos fluxos de formulário (cadastro/login/recuperação), alinhado ao PRD.  
- **i18n:** Português pode ser hardcoded no MVP; extrair ARB depois se a spec pedir.

---

## 7. Backend

- **N/A neste repositório.** A feature de **auth** (PRD) exige backend ou serviço gerenciado; a **spec SDD-1** deve documentar endpoints, modelo de sessão/token e envio de e-mail.  
- **Não assumir:** JWT vs. sessão cookie, Supabase vs. API própria, etc., até constar na spec.

---

## 8. Dados, migrações e performance

- **Cliente:** eventual cache seguro de tokens — usar pacote e padrão definidos na spec (ex.: `flutter_secure_storage`).  
- **Migrações de servidor:** fora do app Flutter; seguir backend da spec.

---

## 9. Segurança (obrigatório)

### 9.1 Segredos e config

- Armazenamento: `.env` / `--dart-define` local; segredos de CI quando existir pipeline.

### 9.2 OWASP / práticas mínimas (auth)

- Não logar senhas nem tokens completos.  
- HTTPS para todas as chamadas de auth em produção.  
- Mensagens de login genéricas (não enumerar e-mails), conforme PRD.

### 9.3 Dados sensíveis em logs e artefatos SDD

- **Nunca** tokens, senhas ou PII real em `sdd/docs/specs/.../proofs` ou commits.  
- Usar `[REDACTED]` e dados fictícios.

---

## 10. Testes e qualidade

### 10.1 Pirâmide e expectativas

| Tipo | Ferramenta | Onde | Notas |
| --- | --- | --- | --- |
| Widget / unitário | `flutter_test` | `sdd/test/` | Alinhar testes com widgets reais após mudanças em `lib/` |
| Integração / E2E | A definir na spec | — | `integration_test` só após decisão explícita |

### 10.2 Mocks e fronteiras

- Mockar cliente HTTP e relógio em testes de lógica de auth quando existir camada injetável.

### 10.3 Definition of Done (técnica)

- [ ] `flutter analyze` sem erros novos introduzidos  
- [ ] `flutter test` passando (ou tarefa explícita para alinhar testes legados)  
- [ ] Sem segredos ou PII em evidências  
- [ ] Dependências novas só com entrada em `pubspec.yaml` e justificativa na spec

---

## 11. Git, CI/CD e releases

### 11.1 Branching

Trunk-based ou feature branches curtas; alinhar ao time (sem política no repo ainda).

### 11.2 Mensagens de commit

Recomendado: [Conventional Commits](https://www.conventionalcommits.org/) em **português ou inglês**, mas **consistente**.

### 11.3 CI

- **Pipeline:** não há YAML de CI no repositório ainda; ao adicionar, espelhar os comandos da seção 3 (`flutter analyze`, `flutter test`).

### 11.4 Deploy

- **Mobile:** lojas / sideload conforme processo do time; não documentado aqui.

---

## 12. Observabilidade

- **N/A** no app atual; crash reporting / analytics quando a spec definir pacote e chaves.

---

## 13. Anti-padrões e erros comuns

1. **Não fazer:** assumir **Node/npm**, **pnpm**, ou pastas `apps/web` — este projeto é **Flutter em `sdd/`**.  
2. **Não fazer:** adicionar pacotes (`pubspec.yaml`) sem necessidade na spec e sem atualizar provas/comandos.  
3. **Não fazer:** implementar backend “invisível” só na pasta `lib/` com segredos hardcoded.  
4. **Não fazer:** deixar `flutter test` vermelho sem tarefa explícita ou correção.  
5. **Não fazer:** expandir escopo além do PRD (ex.: CRUD de alunos) na mesma spec de auth.

---

## 14. Integração explícita com SDD

### 14.1 SDD-1

- Copiar para a spec: comandos seção 3, paths seção 2, limites seções 4 e 7, segurança seção 9.  
- Definir **onde** nasce o backend de auth se o app for só cliente.

### 14.2 SDD-2

- Tarefas devem referenciar arquivos sob `sdd/lib/`, `sdd/test/`, e backend somente se a spec criar esse diretório/repo.

### 14.3 SDD-3 / SDD-4

- Gates: `flutter analyze`, `flutter test`.  
- Proofs: sem PII; screenshots/cURLs com dados fictícios.

---

## 15. Glossário técnico de domínio

| Termo | Significado neste projeto |
| --- | --- |
| **SDD** | Fluxo Spec-Driven Development (prompts SDD-1…4 nesta pasta). |
| **Spec** | Pasta em `sdd/docs/specs/[NN]-spec-.../` com requisitos e provas. |
| **Proof** | Evidência em `proofs/` (comando, screenshot, log redigido). |

---

## 16. Histórico de alterações

| Versão | Data | Autor | Mudança |
| --- | --- | --- | --- |
| 0.1 | — | — | Template original |
| 0.2 | 2026-04-04 | Adriano | Preenchido para repo SDD: Flutter `sdd/`, comandos reais, prompts path, auth sem backend no repo |
| 0.3 | 2026-04-05 | Adriano | Cabeçalho operacional visível; política clara §4.4/§7/§3.4 vs baseline; remoção do guia em comentário HTML; links para PRD e GUIA |

---

### Checklist crítico (o documento está “IA-ready”?)

- [x] Comandos principais são **copiáveis** (`flutter pub get`, `flutter analyze`, `flutter test`)  
- [x] Estrutura de pastas reflete o **estado atual** (`sdd/`, prompts em `lib/project/prompts/`)  
- [x] Limite explícito: **sem backend no repo** até a spec definir  
- [x] Segurança e proofs redigidos (seção 9)  
- [x] Anti-padrões específicos Flutter/repo (seção 13)
