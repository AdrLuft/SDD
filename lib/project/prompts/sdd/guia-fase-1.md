

# GUIA — FASE 1: Configurar contexto da feature

## Como usar

1. Preencha o bloco **"Dados da feature"** abaixo com as informações da sua feature.
2. Anexe `PRD.md` e `TECH-SPEC-IA.md` à conversa (estão na mesma pasta deste arquivo: `lib/prompts/`).
3. Envie este arquivo para a IA e aguarde a proposta de alterações — **não há edição sem sua aprovação**.
4. Confirme as alterações. Só então siga para a Fase 2 (`guia-fase-2.md`).

---

## Instrução para a IA

Você é um **engenheiro sênior de produto e tech lead**. Seu trabalho é atualizar `PRD.md` e `TECH-SPEC-IA.md` para refletir a nova feature descrita abaixo.

### Passo a passo obrigatório

**PASSO 0 — Verificar arquitetura existente (antes de tudo)**

Antes de propor qualquer coisa, execute ESTE algoritmo:

1. O projeto já tem uma pasta `lib/features/`?
   - **SIM** → Use sempre `lib/features/<nome_da_feature>/` com as 3 camadas (data/domain/presentation)
   - **NÃO** → Continue no passo 2
2. O projeto tem outra estrutura clara (ex: `lib/screens/`, `lib/services/`, etc)?
   - **SIM** → Mantenha **exatamente** essa estrutura para a nova feature (respeitar consistência)
   - **NÃO** → Use Clean Architecture conforme seção "🏗️ Regras arquiteturais do projeto" abaixo
3. **Se o usuário deixou explícito** "usar arquitetura X" nos dados da feature → use a X, independente do projeto

⚠️ **Regra de ouro:** Reconhecer o que já existe > impor um padrão novo. Mudança de arquitetura global é decisão do líder, não da IA.

---

**PASSO 2 — Ler e identificar estado atual (NÃO editar ainda)**

Leia integralmente os dois arquivos anexos e identifique:

- `PRD.md`: versão atual (Metadados), último ID de requisito (`PRD-RF-XXX`), última entrada do histórico (§11) e conteúdo atual de cada seção que será alterada.
- `TECH-SPEC-IA.md`: versão atual (Metadados), última entrada do histórico (§16) e conteúdo atual das seções que podem ser afetadas.

**PASSO 3 — Propor alterações (NÃO editar ainda)**

Liste em formato de proposta **todas** as alterações que pretende fazer, seção por seção, mostrando:
- Qual seção será modificada
- O que existe atualmente nessa seção
- O que será adicionado ou ajustado

Aguarde **confirmação explícita** do usuário antes de editar qualquer arquivo.

**PASSO 4 — Executar edições (somente após aprovação)**

Após aprovação: execute as edições conforme os guardrails abaixo e confirme o que foi alterado em cada arquivo.

**Não gere código. Não gere spec. Apenas proponha, aguarde aprovação e então atualize os dois arquivos.**

---

## 🔧 Dados da feature — preencha aqui antes de enviar

**NOME DA FEATURE**
Auth

PROBLEMA QUE RESOLVE
Sem autenticação, qualquer pessoa acessa o sistema sem identidade,
impossibilitando proteger dados, atribuir ações a usuários e evoluir
para permissões por papel no ERP.

PARA QUEM (personas)
Dono da academia, Recepcionista, Instrutor e alunos.

REQUISITOS FUNCIONAIS P0 (obrigatórios)
- O sistema deve permitir login com e-mail e senha
- O sistema deve oferecer acesso ao fluxo de cadastro a partir da tela de login
- O sistema deve oferecer recuperação de senha por e-mail a partir da tela de login
- O sistema deve permitir cadastro com os campos: nome, e-mail, idade, sexo e senha
- O sistema deve manter sessão ativa até logout explícito ou expiração
- O sistema deve permitir logout que encerra a sessão no cliente e invalida no servidor

FLUXO DE NAVEGAÇÃO PÓS-AUTH
  - Após login bem-sucedido: [/home]
  - Após cadastro bem-sucedido: [/login]
  - App aberto com sessão ativa: [redireciona direto para /home, ignora tela de login]
  - App aberto sem sessão: [exibe tela de login]

FORA DE ESCOPO desta feature
- Login social (Google, Apple) — será feature separada se priorizado
- 2FA / biometria — complexidade desnecessária no MVP
- Gestão de usuários por admin (convites, desativação) — feature futura
- Qualquer tela ou módulo além dos fluxos de auth

ESTILO DE UI esperado
Material 3, paleta de cores neutra com um acento primário,
formulários com validação em tempo real nos campos,
feedback visual imediato (snackbar ou indicador inline para erros),
botão primário único por tela, sem modais desnecessários,
estado de loading visível no botão de submit durante requisições

ESPECIFICAÇÃO VISUAL DETALHADA (preencha o máximo possível)

Paleta de cores:
  - Cor primária (hex ou nome): [#1E88E5 / azul]
  - Cor secundária: [#90CAF9 / azul claro]
  - Cor de fundo: [#F5F5F5]
  - Cor de superfície (cards/forms): [#FFFFFF]
  - Cor de erro: [#B00020]
  - Cor de sucesso: [#388E3C]

Tipografia:
  - Fonte principal: [Roboto — padrão Material 3]
  - Tamanho título: [24sp]
  - Tamanho corpo: [16sp]
  - Tamanho caption: [12sp]

Layout e espaçamento:
  - Padding horizontal das telas: [24px]
  - Espaçamento entre seções: [24px]
  - Espaçamento entre campos de form: [16px]
  - Largura máxima do conteúdo (se centralizar em telas grandes): [480px]
  - Border radius padrão (cards, botões, inputs): [12px]

Componentes:
  - Estilo dos inputs: [outlined]
  - Estilo do botão primário: [Filled]
  - Altura mínima dos botões: [52dp]
  - Ícones nos campos de input: [sim, prefixo]
  - AppBar: [sem AppBar]

Feedback visual:
  - Erros de validação: [inline abaixo do campo]
  - Loading durante submit: [loading indicator dentro do botão de submit, botão desabilitado durante requisição]
  - Sucesso: [navega direto]
  - Estado vazio: [não se aplica às telas de auth — padrão reservado para módulos com listagens]

Responsividade:
  - Tela mínima suportada: [360dp largura]
  - Comportamento em tablets: [centralizar form com maxWidth 480dp]
  - Scroll: [telas que podem ultrapassar viewport DEVEM ter scroll]

Acessibilidade:
  - Labels nos campos: [floating ]
  - Contraste mínimo: [WCAG AA = 4.5:1]
  - Semantics em widgets interativos: [obrigatório]

MUDA ALGO NA STACK / ARQUITETURA?
[x] Sim

Novo pacote: supabase_flutter — cliente oficial Supabase para auth e BaaS
Novo pacote: get — gerenciamento de estado e navegação (GetX)
Novo pacote: crypto — hash local de dados antes de envio (campos PII como CPF)
Novo pacote: validatorless — validação declarativa de formulários
Novo pacote: flutter_secure_storage — persistência segura de tokens de sessão

Nova pasta/camada:
lib/features/auth/ — feature de auth (Clean Architecture)
lib/features/auth/data/
lib/features/auth/domain/
lib/features/auth/presentation/

Nova decisão arquitetural (ADR):
Adotar Clean Architecture como padrão do projeto:
separação em data / domain / presentation por feature.
Motivo: facilita manutenção, testabilidade e expansão para outros módulos do ERP.

Novo backend/serviço externo:
Supabase — BaaS para auth (cadastro, login, recuperação de senha, sessão).
SDK: supabase_flutter.

Novas variáveis de ambiente:
SUPABASE_URL — URL do projeto Supabase
SUPABASE_ANON_KEY — chave pública anon do Supabase

---

## 🏗️ Regras arquiteturais do projeto — leia antes de propor qualquer alteração

> **Este bloco define o padrão CANÔNICO do projeto.** Se o projeto já tem estrutura diferente ativa (com código funcionando), o PASSO 0 pode honrar essa estrutura. Mas SE você estiver começando um novo projeto OU migrando para o padrão: use ESTAS regras como lei.

### Estrutura de pastas obrigatória (Clean Architecture por feature)

Toda feature do projeto segue **Clean Architecture** organizada dentro de `lib/features/`. A estrutura canônica é:

```
lib/
  features/
    <nome_da_feature>/
      data/              ← Repositórios concretos, data sources, models (DTO/JSON)
      domain/            ← Entidades, use cases, contratos de repositório (interfaces)
      presentation/      ← Pages (telas), Controllers/Bindings (GetX), widgets específicos
```

**Regras de estrutura:**

- Cada feature vive em `lib/features/<nome>/` e DEVE conter as 3 camadas: `data/`, `domain/`, `presentation/`
- Código compartilhado entre features (widgets globais, temas, utils) fica em `lib/core/`
- O entry point do app (`main.dart`) fica em `lib/` e apenas inicializa o app e define rotas
- Testes seguem a mesma árvore: `test/features/<nome>/data/`, `test/features/<nome>/domain/`, `test/features/<nome>/presentation/`

### 🚫 Anti-padrões proibidos — a IA NUNCA deve

| # | Anti-padrão | Por que é proibido |
|---|---|---|
| 1 | Criar arquivos de feature em `bin/` | `bin/` é APENAS para scripts CLI auxiliares do Dart, nunca para lógica de app |
| 2 | Criar telas soltas em `lib/screens/` | Toda tela pertence a `lib/features/<nome>/presentation/` — `lib/screens/` não existe na arquitetura |
| 3 | Criar código de feature fora de `lib/features/` | Repositórios, use cases, controllers — tudo fica dentro da feature correspondente |
| 4 | Misturar camadas (ex.: chamar data source direto da presentation) | A presentation chama use cases (domain), que chamam repositórios (data) — nunca pular camadas |
| 5 | Gerar um único arquivo monolítico com toda a feature | Cada classe em seu próprio arquivo, organizada na camada correta |
| 6 | Adicionar pacote ao `pubspec.yaml` sem justificativa na spec | Todo pacote novo precisa estar declarado nos dados da feature ou na spec aprovada |
| 7 | Criar tela sem `SingleChildScrollView` ou `ListView` | Todo layout com conteúdo dinâmico DEVE ter scroll para evitar overflow |
| 8 | Criar botão/campo sem altura mínima de 48dp | Touch targets pequenos prejudicam usabilidade mobile |
| 9 | Criar tela sem implementar TODOS os estados (loading, erro, sucesso, vazio) | Telas incompletas geram bugs visuais e UX ruim |
| 10 | Usar apenas placeholder como label de campo (sem label flutuante/fixa) | Acessibilidade exige labels permanentemente visíveis |
| 11 | Navegar para rota protegida sem verificar sessão ativa | 
Toda rota autenticada DEVE passar por um AuthGuard/middleware GetX antes de renderizar — nunca confiar só no estado local |

### Camadas e responsabilidades

| Camada | Contém | Depende de |
|---|---|---|
| `presentation/` | Pages (telas), Controllers (GetX), Bindings, Widgets locais | `domain/` (use cases e entidades) |
| `domain/` | Entities, Use Cases, Repository contracts (abstract classes) | Nada — é o núcleo isolado |
| `data/` | Repository implementations, Data Sources (remote/local), Models (DTO) | `domain/` (implementa os contratos) |

### Como esta seção se propaga

- Na **Fase 1** (este arquivo): o PASSO 0 identifica a arquitetura real do projeto; a IA usa essas regras (ou a estrutura honrada do PASSO 0) para propor alterações em `TECH-SPEC-IA.md` §2.1, §4.1 e §4.4
- Na **Fase 2** (`guia-fase-2.md`): SDD-1 gera a spec respeitando a arquitetura confirmada; SDD-2 gera tasks com paths corretos; SDD-3 implementa nos diretórios certos; SDD-4 valida que a estrutura foi mantida
- Se algo contradizer a arquitetura confirmada em qualquer etapa, **PARE e pergunte ao usuário** antes de continuar

---

## 📋 Guardrails obrigatórios da Fase 1

> **Estes guardrails são instruções PARA A IA. Não altere este bloco.**

### PRD.md — o que atualizar

| Seção | Ação permitida | Regra |
| --- | --- | --- |
| §3 (personas) | Adicionar personas novas às já existentes | Nunca remover personas de features anteriores |
| §4 (escopo funcional) | Adicionar requisitos com novos IDs sequenciais (`PRD-RF-XXX`) | Nunca reutilizar ou alterar IDs existentes |
| §5 (UX) | Adicionar bloco de UX da nova feature com base na ESPECIFICAÇÃO VISUAL DETALHADA dos dados da feature | Manter blocos de features anteriores. Transferir cores, tipografia, espaçamento e componentes definidos pelo usuário |
| §6 (fora de escopo) | Adicionar itens de não-escopo desta feature | Manter os itens de features anteriores |
| §8 (riscos / questões em aberto) | Adicionar novos riscos se existirem | Nunca remover riscos anteriores não resolvidos |
| §11 (histórico) | Adicionar nova linha com versão incrementada, data de hoje e resumo | Nunca editar linhas existentes do histórico |

⚠️ **Seção 1 (contexto e problema) e Seção 2 (objetivos) NÃO são sobrescritas.** O PRD é um documento vivo que acumula o histórico do produto inteiro. Cada feature adiciona ao documento — nunca apaga o que veio antes.

⚠️ **ESPECIFICAÇÃO VISUAL DETALHADA é obrigatória.** Se o campo estiver vazio ou com apenas "[ex.: ...]", a IA DEVE perguntar ao usuário antes de propor alterações. UI genérica ou "Material 3 padrão" sem detalhamento gera telas simplistas.

### PRD.md — o que NÃO alterar (nunca)

- Estrutura, títulos e formato das seções
- IDs de requisitos já existentes (`PRD-RF-XXX`)
- Metadados do produto (nome, autores, data de criação)
- Conteúdo de features anteriores em qualquer seção
- Métricas numéricas — estas são definidas pelo humano, não pela IA; deixe o campo como `[a definir]` se não houver valor explícito nos dados da feature

### TECH-SPEC-IA.md — o que atualizar (SOMENTE se "Sim" marcado em "MUDA ALGO NA STACK?")

> A IA deve aplicar as regras da seção **"🏗️ Regras arquiteturais do projeto"** acima ao propor alterações nestas seções. Paths, camadas e anti-padrões vêm de lá.

| Seção | Ação |
| --- | --- |
| §2.1 (Mapa do repositório) | Adicionar novas pastas/camadas **somente se declaradas nos dados da feature** |
| §3.4 (Variáveis de ambiente) | Adicionar novas variáveis |
| §4.1 (Estilo arquitetural) | Atualizar **somente se** a feature introduzir novo padrão de arquitetura (ex.: Clean Architecture) |
| §4.4 (ADR) | Adicionar nova linha de decisão arquitetural |
| §6 (Frontend Flutter) | Atualizar APENAS se mudar padrão de estado, roteamento ou componentes globais |
| §7 (Backend) | Preencher somente se a feature introduzir novo serviço ou BaaS |
| §13 (Anti-padrões) | Adicionar regra APENAS se a feature introduzir novo risco recorrente e documentável |
| §16 (Histórico) | Adicionar linha com versão incrementada e data de hoje |

### TECH-SPEC-IA.md — o que NÃO alterar (nunca)

- Seções 1, 5, 9, 10, 11 — são regras fixas do projeto
- Comandos canônicos da seção 3 — só mudam se o CI/CD mudar, decisão humana
- Paths existentes no mapa do repositório (seção 2) — só adicionar, nunca remover

---

## ✅ Entrega esperada ao final da Fase 1

A IA deve:

1. **Proposta** (antes de editar): lista de todas as alterações planejadas por arquivo e seção
2. **Aguardar confirmação** explícita do usuário
3. **Após aprovação**: executar as edições
4. **Relatório final**: confirmar cada campo efetivamente alterado em `PRD.md` e em `TECH-SPEC-IA.md` (ou informar "nenhuma alteração necessária" para o TECH-SPEC-IA se a stack não mudou)
5. **Aguardar confirmação final** do usuário antes de seguir para a Fase 2

⚠️ Se os arquivos gerados estiverem incorretos: corrija manualmente e reenvie a Fase 1 com a nota `[CORREÇÃO: reprocesse apenas a seção X]`. Não avance para a Fase 2 enquanto os dois arquivos não estiverem corretos e aprovados.

