

<!-- 
MODELOS DE IA PRECISAM IGNORAR ESSES COMENTÁRIOS!!!

| Campo                    | O que você coloca                                              |
| ------------------------ | -------------------------------------------------------------- |
| NOME DA FEATURE          | Nome curto da feature                                          |
| PROBLEMA QUE RESOLVE     | Por que essa feature existe, dor real do usuário               |
| PARA QUEM (personas)     | Quem vai usar: Recepcionista, Dono, etc.                       |
| REQUISITOS FUNCIONAIS P0 | O que o sistema deve fazer — comportamentos obrigatórios       |
| FORA DE ESCOPO           | O que não será feito nessa entrega e por quê                   |
| ESTILO DE UI esperado    | ← Aqui entram os detalhes de UI                                |
| MUDA ALGO NA STACK?      | Se adicionar pacote, serviço, pasta nova, variável de ambiente |-->






 GUIA — COMO CRIAR UMA FEATURE (DUAS FASES)
Como usar:

Preencha o bloco de dados da Fase 1 abaixo com as informações da sua feature.

Anexe PRD.md e TECH-SPEC-IA.md à conversa.

Envie a Fase 1 para a IA e aguarde a proposta de alterações — não há edição sem sua aprovação.

Confirme as alterações. Só então siga para a Fase 2.

FASE 1 — Configurar contexto da feature
Instrução para a IA
Você é um engenheiro sênior de produto e tech lead.

Antes de qualquer ação, execute obrigatoriamente os três passos abaixo:

Leia integralmente o arquivo PRD.md anexo. Identifique: versão atual, último ID de requisito existente (PRD-RF-XXX), última entrada do histórico de alterações (seção 11) e o conteúdo atual de cada seção que será alterada.

Leia integralmente o arquivo TECH-SPEC-IA.md anexo. Identifique: versão atual, última entrada do histórico (seção 16) e o conteúdo atual das seções que podem ser afetadas.

Não edite nenhum arquivo ainda. Apenas leia e prossiga para o próximo passo.

Após ler os dois arquivos, liste em formato de proposta todas as alterações que pretende fazer, seção por seção, mostrando:

Qual seção será modificada

O que existe atualmente nessa seção

O que será adicionado ou ajustado

Aguarde confirmação explícita do usuário antes de editar qualquer arquivo.

Somente após aprovação: execute as edições conforme os guardrails abaixo e confirme o que foi alterado em cada arquivo.

Não gere código. Não gere spec. Apenas proponha, aguarde aprovação e então atualize os dois arquivos.

🔧 Dados da feature — preencha aqui antes de enviar
NOME DA FEATURE
Auth

PROBLEMA QUE RESOLVE
Sem autenticação, qualquer pessoa acessa o sistema sem identidade,
impossibilitando proteger dados, atribuir ações a usuários e evoluir
para permissões por papel no ERP.

PARA QUEM (personas)
Dono da academia, Recepcionista, Instrutor

REQUISITOS FUNCIONAIS P0 (obrigatórios)
- O sistema deve permitir login com e-mail e senha
- O sistema deve oferecer acesso ao fluxo de cadastro a partir da tela de login
- O sistema deve oferecer recuperação de senha por e-mail a partir da tela de login
- O sistema deve permitir cadastro com os campos: nome, e-mail, idade, sexo e senha
- O sistema deve manter sessão ativa até logout explícito ou expiração
- O sistema deve permitir logout que encerra a sessão no cliente e invalida no servidor

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

MUDA ALGO NA STACK / ARQUITETURA?
[x] Sim

Novo pacote: supabase_flutter — cliente oficial Supabase para auth e BaaS
Novo pacote: get — gerenciamento de estado e navegação (GetX)
Novo pacote: crypto — hash de dados sensíveis se necessário
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
📋 Guardrails obrigatórios da Fase 1 — não altere este bloco
PRD.md — o que atualizar
Seção	Ação permitida	Regra
§3 (personas)	Adicionar personas novas às já existentes	Nunca remover personas de features anteriores
§4 (escopo funcional)	Adicionar requisitos com novos IDs sequenciais (PRD-RF-XXX)	Nunca reutilizar ou alterar IDs existentes
§5 (UX)	Adicionar bloco de UX da nova feature	Manter blocos de features anteriores
§6 (fora de escopo)	Adicionar itens de não-escopo desta feature	Manter os itens de features anteriores
§8 (riscos / questões em aberto)	Adicionar novos riscos se existirem	Nunca remover riscos anteriores não resolvidos
§11 (histórico)	Adicionar nova linha com versão incrementada, data de hoje e resumo	Nunca editar linhas existentes do histórico
⚠️ Seção 1 (contexto e problema) e Seção 2 (objetivos) NÃO são sobrescritas. O PRD é um documento vivo que acumula o histórico do produto inteiro. Cada feature adiciona ao documento — nunca apaga o que veio antes.

PRD.md — o que NÃO alterar (nunca)
Estrutura, títulos e formato das seções

IDs de requisitos já existentes (PRD-RF-XXX)

Metadados do produto (nome, autores, data de criação)

Conteúdo de features anteriores em qualquer seção

Métricas numéricas — estas são definidas pelo humano, não pela IA; deixe o campo como [a definir] se não houver valor explícito nos dados da feature

TECH-SPEC-IA.md — o que atualizar (SOMENTE se "Sim" marcado acima)
Seção	Ação
§3.4 (Variáveis de ambiente)	Adicionar novas variáveis
§4.4 (ADR)	Adicionar nova linha de decisão arquitetural
§6 (Frontend Flutter)	Atualizar APENAS se mudar padrão de estado, roteamento ou componentes globais
§7 (Backend)	Preencher somente se a feature introduzir novo serviço ou BaaS
§13 (Anti-padrões)	Adicionar regra APENAS se a feature introduzir novo risco recorrente e documentável
§16 (Histórico)	Adicionar linha com versão incrementada e data de hoje
TECH-SPEC-IA.md — o que NÃO alterar (nunca)
Seções 1, 5, 9, 10, 11 — são regras fixas do projeto

Comandos canônicos da seção 3 — só mudam se o CI/CD mudar, decisão humana

Mapa do repositório (seção 2) — só muda se uma nova pasta física for criada no repo

✅ Entrega esperada ao final da Fase 1
A IA deve:

Proposta (antes de editar): lista de todas as alterações planejadas por arquivo e seção

Aguardar confirmação explícita do usuário

Após aprovação: executar as edições

Relatório final: confirmar cada campo efetivamente alterado em PRD.md e em TECH-SPEC-IA.md (ou informar "nenhuma alteração necessária" para o TECH-SPEC-IA se a stack não mudou)

Aguardar confirmação final do usuário antes de seguir para a Fase 2

⚠️ Se os arquivos gerados estiverem incorretos: corrija manualmente e reenvie a Fase 1 com a nota [CORREÇÃO: reprocesse apenas a seção X]. Não avance para a Fase 2 enquanto os dois arquivos não estiverem corretos e aprovados.








____________________________________________________________________________________

########################################  FASE 2  ###################################################################

FASE 2 — Executar o fluxo SDD
Instrução para a IA
Os arquivos PRD.md e TECH-SPEC-IA.md já foram atualizados e aprovados na Fase 1. Execute agora o fluxo SDD estritamente na ordem abaixo. Em cada etapa, os dois arquivos devem estar anexados à conversa.

⚠️ Checklist pré-voo — confirme ANTES de iniciar qualquer etapa
 PRD.md atualizado e aprovado na Fase 1 está anexado nesta conversa

 TECH-SPEC-IA.md atualizado (ou confirmado sem alterações) está anexado nesta conversa

 A Fase 1 foi concluída com confirmação explícita do usuário

Não inicie nenhuma etapa SDD sem esses três itens confirmados.

ETAPA 1 → SDD-1-generate-spec.md
O que acontece nesta etapa:

Abra (ou carregue) o arquivo SDD-1-generate-spec.md junto com PRD.md e TECH-SPEC-IA.md

Descreva a feature para a IA usando os dados preenchidos na Fase 1

A IA irá: avaliar escopo, criar diretório ./docs/specs/[NN]-spec-[nome]/ e gerar um arquivo de perguntas [NN]-questions-1-[nome].md

Responda as perguntas diretamente no arquivo gerado e salve

Pode haver múltiplas rodadas de perguntas (questions-1, questions-2...) — responda todas antes de avançar

A IA gera a spec. Você aprova: "Esta spec reflete seus requisitos? Os limites de escopo estão corretos?"

Só avance para a Etapa 2 após aprovação explícita da spec

ETAPA 2 → SDD-2-generate-task-list-from-spec.md
O que acontece nesta etapa:

Abra SDD-2-generate-task-list-from-spec.md apontando para a spec aprovada na Etapa 1

A IA gera as tarefas pai primeiro e apresenta para aprovação

Confirme as tarefas pai antes de a IA gerar as subtarefas

Só avance para a Etapa 3 após a lista de tarefas estar completa e aprovada

ETAPA 3 → SDD-3-manage-tasks.md
O que acontece nesta etapa:

Abra SDD-3-manage-tasks.md com a lista de tarefas aprovada

A IA executa as tarefas na ordem definida, roda testes e lint após cada tarefa, e grava artefatos de prova

Acompanhe a execução — não pule validações intermediárias

Só avance para a Etapa 4 após todas as tarefas estarem concluídas e com provas gravadas

ETAPA 4 → SDD-4-validate-spec-implementation.md
O que acontece nesta etapa:

Abra SDD-4-validate-spec-implementation.md

A IA valida a implementação contra a spec aprovada na Etapa 1 e gera relatório de validação

Se reprovar (total ou parcialmente):

Corrija os itens reprovados conforme o relatório

Revalide executando a Etapa 4 novamente

Se a correção exigir mudança de escopo ou arquitetura → volte à Fase 1 antes de continuar

A feature só está concluída quando o SDD-4 emitir aprovação

🚫 Guardrails da Fase 2 — a IA NUNCA deve
Pular etapas ou executar qualquer SDD fora da ordem 1 → 2 → 3 → 4

Implementar código antes de a spec estar aprovada (Etapa 1 concluída)

Adicionar pacotes ao pubspec.yaml sem justificativa registrada na spec aprovada

Assumir Node, npm, Laravel, Firebase ou qualquer backend/serviço não definido explicitamente na spec

Commitar tokens, senhas, chaves de API ou PII real nos artefatos de prova

Expandir o escopo além do que está na seção §4 do PRD atualizado na Fase 1

Definir métricas numéricas de sucesso — isso é responsabilidade do humano