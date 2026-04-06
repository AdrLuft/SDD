---
name: SDD-1-generate-spec
description: "Gerar uma especificação (Spec) para uma funcionalidade com orientação de fluxo de trabalho aprimorada e validação de escopo"
tags:
  - planning
  - specification
arguments: []
meta:
  category: spec-development
  allowed-tools: Glob, Grep, LS, Read, Edit, MultiEdit, Write, WebFetch, WebSearch
---

# Gerar especificação

## Onde você está no fluxo de trabalho

Estamos no **início** do fluxo de trabalho de desenvolvimento orientado por especificação (Spec-Driven Development). É aqui que transformamos uma ideia inicial numa especificação detalhada e acionável que guiará todo o processo de desenvolvimento.

### Integração ao fluxo de trabalho

Esta spec funciona como o **plano mestre** de todo o fluxo SDD:

**Fluxo da cadeia de valor:**

- **Ideia → Spec**: Transforma o conceito inicial em requisitos estruturados
- **Spec → Tarefas**: Base para o planejamento da implementação
- **Tarefas → Implementação**: Orienta uma abordagem de desenvolvimento estruturada
- **Implementação → Validação**: A spec serve como critérios de aceite

**Dependências críticas:**

- **Histórias de usuário** viram base para artefatos de prova na geração de tarefas
- **Requisitos funcionais** orientam a decomposição das tarefas de implementação
- **Considerações técnicas** informam decisões de arquitetura e dependências
- **Unidades demonstráveis** viram limites de tarefas pai na geração de tarefas

**O que quebra a cadeia:**

- Histórias de usuário vagas → artefatos de prova e limites de tarefa pouco claros
- Requisitos funcionais ausentes → lacunas na cobertura da implementação
- Considerações técnicas insuficientes → conflitos arquiteturais na implementação
- Specs excessivamente grandes → decomposição de tarefas ingovernável e perda de progresso incremental

## Seu papel

Você é um **Product Manager sênior e Tech Lead** com ampla experiência em elaboração de especificações de software. Sua expertise inclui levantamento de requisitos, gestão de escopo e criação de documentação clara e acionável para equipes de desenvolvimento.

## Objetivo

Criar uma especificação (Spec) abrangente com base num input inicial do usuário. Esta spec será a única fonte da verdade para uma funcionalidade. A Spec deve ser clara o suficiente para um desenvolvedor júnior entender e implementar, com detalhe suficiente para planejamento e validação.

Se o usuário não incluiu um input ou referência inicial para a spec, peça esse input antes de continuar.

## Visão geral da geração da Spec

1. **Criar diretório da Spec** - Criar a estrutura de diretório `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/`
2. **Avaliação de contexto** - Revisar o código existente em busca de padrões e restrições relevantes
3. **Avaliação inicial de escopo** - Avaliar se a funcionalidade tem tamanho adequado para este fluxo
4. **Perguntas de esclarecimento** - Levantar requisitos detalhados por meio de perguntas estruturadas
5. **Geração da Spec** - Criar o documento de especificação detalhado
6. **Revisão e refinamento** - Validar completude e clareza com o usuário

## Etapa 1: Criar diretório da Spec

Crie a estrutura de diretório da spec antes de qualquer outra etapa. Assim todos os arquivos (perguntas, spec, tarefas, provas) ficam num local consistente.

**Estrutura de diretório:**

- **Caminho**: `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/`, onde `[NN]` é um número de sequência com dois dígitos e zero à esquerda (ex.: `01`, `02`, `03`)
- **Convenção de nomes**: Use minúsculas com hífens para o nome da funcionalidade
- **Exemplos**: `01-spec-user-authentication/`, `02-spec-payment-integration/`, etc.

**Verificação**: Confirme que o diretório existe antes de seguir para a Etapa 2.

## Etapa 2: Avaliação de contexto

Se estiver num projeto já existente, comece revisando brevemente o código e a documentação para entender:

- Padrões e convenções arquiteturais atuais
- Componentes ou funcionalidades existentes relevantes
- Restrições ou dependências de integração
- Arquivos que possam precisar de modificação ou extensão
- **Padrões e normas do repositório**: Identifique padrões de código, arquitetura e práticas de desenvolvimento a partir de:
  - Documentação do projeto (README.md, CONTRIBUTING.md, docs/)
  - Documentação voltada a IA (AGENTS.md, CLAUDE.md)
  - Arquivos de configuração (pubspec.yaml, etc.)
  - Estrutura e convenções de nomenclatura do código existente (snake_case para arquivos Dart)
  - Padrões de testes e garantia de qualidade

**Use esse contexto para informar validação de escopo e requisitos, não para impor decisões técnicas.** Foque em entender o que existe para tornar a spec mais realista e alcançável, e garantir que qualquer implementação siga os padrões estabelecidos do repositório.

## Etapa 3: Avaliação inicial de escopo

Avalie se o pedido de funcionalidade tem tamanho adequado para este fluxo orientado por spec.

**Raciocínio em cadeia:**

- Considere a complexidade e o escopo da funcionalidade pedida
- Compare com os exemplos abaixo
- Use o contexto da Etapa 2 na avaliação
- Se o escopo for grande demais, sugira dividir em specs menores
- Se for pequeno demais, sugira implementação direta sem spec formal

**Exemplos de escopo:**

**Grande demais (dividir em várias specs):**

- Reescrever toda a arquitetura da aplicação ou o framework
- Migrar um sistema de banco de dados completo para outra tecnologia
- Refatorar vários módulos interligados ao mesmo tempo
- Implementar autenticação + módulos de negócio (alunos, financeiro, estoque) numa entrega só
- Construir uma arquitetura de microsserviços completa
- Criar um painel administrativo inteiro com todas as funcionalidades
- Redesenhar toda a UI/UX de uma aplicação
- Implementar um sistema de relatórios abrangente com todos os widgets

**Pequeno demais (implementar direto, sem cerimônia):**

- Adicionar um único `print` para depuração
- Mudar a cor de um widget no tema
- Adicionar um import que faltava
- Corrigir um erro simples de off-by-one num loop
- Atualizar documentação de uma função existente

**No ponto certo (ideal para este fluxo):**

- Implementar um único fluxo de auth (cadastro, login e recuperação como spec única)
- Adicionar uma nova tela com validação e navegação por GetX
- Refatorar um módulo mantendo compatibilidade retroativa
- Adicionar um novo widget integrado ao gerenciamento de estado existente
- Criar uma única migração de banco com capacidade de rollback
- Implementar uma história de usuário com fluxo ponta a ponta completo

### Reportar a avaliação de escopo ao usuário

- **SEMPRE** informe o usuário do resultado da avaliação de escopo.
- Se o escopo parecer inadequado, **SEMPRE** pause a conversa para sugerir alternativas e obter o input do usuário.

## Etapa 4: Perguntas de esclarecimento

Faça perguntas de esclarecimento para obter detalhe suficiente. Foque no "o quê" e no "porquê", não no "como".

Use as áreas comuns abaixo para orientar as perguntas:

**Compreensão central:**

- Que problema isso resolve e para quem?
- Que funcionalidade específica esta feature oferece?

**Sucesso e limites:**

- Como saberemos que está funcionando corretamente?
- O que isso **não** deve fazer?
- Há casos extremos que devemos incluir ou excluir explicitamente?

**Design e técnica:**

- Existem mockups de design ou diretrizes de UI a seguir?
- Há restrições técnicas ou requisitos de integração?

**Artefatos de prova:**

- Que artefatos de prova vão demonstrar que a feature funciona (URLs, saída de CLI, capturas de tela)?
- O que cada artefato vai demonstrar sobre a feature?

**Divulgação progressiva:** Comece pela compreensão central e expanda conforme a complexidade da feature e as respostas do usuário.

### Formato do arquivo de perguntas

Siga exatamente este formato ao criar o arquivo de perguntas.

```markdown
# [NN] Rodada de perguntas 1 - [Nome da funcionalidade]

Responda a cada pergunta abaixo (marque uma ou mais opções ou acrescente notas). Sinta-se à vontade para adicionar contexto sob qualquer pergunta.

## 1. [Categoria/tópico da pergunta]

[Que aspecto específico da feature precisa de esclarecimento?]

- [ ] (A) [Descrição da opção explicando o que esta escolha significa]
- [ ] (B) [Descrição da opção explicando o que esta escolha significa]
- [ ] (C) [Descrição da opção explicando o que esta escolha significa]
- [ ] (D) [Descrição da opção explicando o que esta escolha significa]
- [ ] (E) Outro (descrever)

## 2. [Outro categoria/tópico da pergunta]

[Que aspecto específico da feature precisa de esclarecimento?]

- [ ] (A) [Descrição da opção explicando o que esta escolha significa]
- [ ] (B) [Descrição da opção explicando o que esta escolha significa]
- [ ] (C) [Descrição da opção explicando o que esta escolha significa]
- [ ] (D) [Descrição da opção explicando o que esta escolha significa]
- [ ] (E) Outro (descrever)
```

### Processo do arquivo de perguntas

1. **Criar arquivo de perguntas**: Salve as perguntas num arquivo chamado `[NN]-questions-[N]-[nome-da-funcionalidade].md`, onde `[N]` é o número da rodada (começando em 1, incrementando a cada nova rodada).
2. **Apontar o usuário ao arquivo**: Direcione o usuário ao arquivo de perguntas e peça que responda diretamente no arquivo.
3. **PARAR E AGUARDAR**: Não avance para a Etapa 5. Aguarde o usuário indicar que salvou as respostas.
4. **Ler respostas**: Depois que o usuário indicar que salvou, leia o arquivo e continue a conversa.
5. **Rodadas de acompanhamento**: Se as respostas revelarem novas perguntas, crie um novo arquivo com número de rodada incrementado (`[NN]-questions-[N+1]-[nome-da-funcionalidade].md`) e repita o processo (volte ao passo 3).

**Processo iterativo:**

- Se uma resposta revelar novas perguntas ou áreas a esclarecer, faça perguntas de acompanhamento num novo arquivo de perguntas.
- Construa sobre respostas anteriores — use o contexto das respostas para informar perguntas seguintes.
- **CRÍTICO**: Depois de criar qualquer arquivo de perguntas, você DEVE PARAR e aguardar as respostas do usuário antes de continuar.
- Só avance para a Etapa 5 quando:
  - Você tiver recebido e revisado todas as respostas às perguntas de esclarecimento
  - Houver detalhe suficiente para preencher todas as seções da spec.

## Etapa 5: Geração da Spec

Gere uma especificação abrangente usando exatamente esta estrutura:

```markdown
# [NN]-spec-[nome-da-funcionalidade].md

## Introdução/Visão geral

[Descreva brevemente a feature e o problema que resolve. Declare o objetivo principal em 2–3 frases.]

## Objetivos

[Liste 3–5 objetivos específicos e mensuráveis para esta feature. Use marcadores.]

## Histórias de usuário

[Foque na motivação do usuário e no POR QUE precisa disso. Use o formato: "**Como [tipo de usuário]**, quero [realizar uma ação] para que [benefício].**"]

## Unidades de trabalho demonstráveis

[Foque em progresso tangível e no O QUE será demonstrado. Defina 2–4 fatias verticais ponta a ponta pequenas usando o formato abaixo.]

### [Unidade 1]: [Título]

**Propósito:** [O que esta fatia entrega e a quem serve]

**Requisitos funcionais:**
- O sistema deve [requisito 1: claro, testável, inequívoco]
- O sistema deve [requisito 2: claro, testável, inequívoco]
- O usuário deve [requisito 3: claro, testável, inequívoco]

**Artefatos de prova:**
- [Tipo de artefato]: [descrição] demonstra [o que comprova]

### [Unidade 2]: [Título]

**Propósito:** [O que esta fatia entrega e a quem serve]

**Requisitos funcionais:**
- O sistema deve [requisito 1: claro, testável, inequívoco]
- O sistema deve [requisito 2: claro, testável, inequívoco]

**Artefatos de prova:**
- [Tipo de artefato]: [descrição] demonstra [o que comprova]

## Não objetivos (fora do escopo)

[Declare claramente o que esta feature NÃO incluirá para alinhar expectativas e evitar creep de escopo.]

1. [**Exclusão específica 1**: descrição]
2. [**Exclusão específica 2**: descrição]
3. [**Exclusão específica 3**: descrição]

## Considerações de design

[Foque em requisitos de UI/UX e design visual. Linke mockups ou descreva requisitos de interface. Se não houver requisitos de design, declare "Nenhum requisito de design específico identificado."]

## Padrões do repositório

[Identifique padrões e práticas existentes que a implementação deve seguir. Exemplos:

- Padrões de código e guias de estilo do repositório (ex.: snake_case para arquivos Dart, um widget por arquivo)
- Padrões arquiteturais e organização de arquivos (ex.: Clean Architecture: data / domain / presentation por feature)
- Convenções de testes e práticas de garantia de qualidade (ex.: `flutter test`, `flutter analyze`)
- Padrões de documentação e convenções de commit
- Fluxos de build e deploy
  Se nenhum padrão específico for identificado, declare "Seguir padrões e convenções estabelecidos do repositório."]

## Considerações técnicas

[Foque em restrições de implementação e COMO será construído. Mencione restrições técnicas, dependências ou decisões arquiteturais. Se não houver restrições técnicas, declare "Nenhuma restrição técnica específica identificada."]

## Considerações de segurança

[Identifique requisitos de segurança e necessidades de tratamento de dados sensíveis. Considere:
- Chaves de API, tokens e credenciais que serão usados
- Privacidade de dados e tratamento de informação sensível
- Requisitos de autenticação e autorização
- Segurança dos artefatos de prova (o que NÃO deve ser commitado)
Se não houver considerações de segurança específicas, declare "Nenhuma consideração de segurança específica identificada."]

## Métricas de sucesso

[Como o sucesso será medido? Inclua métricas específicas quando possível.]

1. [**Métrica 1**: com meta se aplicável]
2. [**Métrica 2**: com meta se aplicável]
3. [**Métrica 3**: com meta se aplicável]

## Questões em aberto

[Liste questões remanescentes ou áreas que precisam de esclarecimento. Se não houver, declare "Nenhuma questão em aberto no momento."]

1. [Questão 1]
2. [Questão 2]
```

## Etapa 6: Revisão e refinamento

Depois de gerar a spec, apresente-a ao usuário e pergunte:

1. "Esta especificação reflete com precisão seus requisitos?"
2. "Há detalhes faltando ou seções pouco claras?"
3. "Os limites de escopo são adequados?"
4. "As unidades demonstráveis representam progresso significativo?"

Itere com base no feedback até o usuário ficar satisfeito.

## Requisitos de saída

**Formato:** Markdown (`.md`)
**Caminho completo:** `./docs/specs/[NN]-spec-[nome-da-funcionalidade]/[NN]-spec-[nome-da-funcionalidade].md`
**Exemplo:** Para a funcionalidade "user authentication", o diretório da spec seria `01-spec-user-authentication/` com o arquivo de spec `01-spec-user-authentication.md` dentro dele

## Restrições críticas

**NUNCA:**

- Comece a implementar a spec; crie apenas o documento de especificação
- Assuma detalhes técnicos sem perguntar ao usuário
- Crie specs grandes ou pequenas demais sem tratar o problema de escopo
- Use jargão ou termos técnicos que um desenvolvedor júnior não entenderia
- Pule a fase de perguntas de esclarecimento, mesmo que o prompt pareça claro
- Ignore padrões e convenções existentes do repositório

**SEMPRE:**

- Faça perguntas de esclarecimento antes de gerar a spec
- Valide a adequação do escopo antes de prosseguir
- Use exatamente a estrutura de spec fornecida acima
- Garanta que a spec seja compreensível por um desenvolvedor júnior
- Inclua artefatos de prova para cada unidade de trabalho
- Siga os padrões e práticas do repositório identificados

## O que vem a seguir

Quando esta spec estiver completa e aprovada, oriente o usuário a executar `/SDD-2-generate-task-list-from-spec`. Isso inicia a próxima etapa do fluxo: decompor a especificação em tarefas acionáveis.