Exemplo completo de TaskCard?
# [T04] Implement card CRUD API endpoints

## Contexto
Faz parte da feature de Kanban Board definida em `docs/specs/01-spec-kanban/spec.md`.
Depende de: T01 (schema criado). Bloqueada por: nenhuma.

## Objetivo
Criar os endpoints REST para criação, leitura, atualização e remoção de cards,
com suporte a concorrência otimista.

## Escopo (o que ESTÁ incluso)
- POST /cards, GET /cards/:id, PUT /cards/:id, DELETE /cards/:id
- Validação de input (título obrigatório, columnId válido)
- Versionamento para concorrência otimista (campo `version`)

## Fora de Escopo (o que NÃO está incluso)
- Autenticação/autorização (T07)
- Notificações em tempo real (T09)

## Arquivos Afetados
- `src/api/cards.controller.ts` → criar
- `src/api/cards.service.ts` → criar
- `tests/api/cards.test.ts` → criar

## Critérios de Conclusão (Acceptance Criteria)
- [ ] POST retorna 201 com o card criado
- [ ] GET retorna 404 para card inexistente
- [ ] PUT com `version` desatualizada retorna 409 (conflito)
- [ ] DELETE remove o card e retorna 204

## Artefato de Prova
Todos os testes em `tests/api/cards.test.ts` passando com `npm test`.

## Boundaries
- ✅ Sempre: rodar testes após implementar
- ⚠️ Perguntar antes: mudanças no schema do banco
- 🚫 Nunca: modificar migrations já aplicadas