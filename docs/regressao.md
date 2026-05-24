

## Regressão 1 — BUG-003 (Registro como ADMIN)

**Cenário simulado:** O dev corrigiu o endpoint de registro
para ignorar o campo role vindo do cliente.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-001 | Login válido | Garantir que o login não foi afetado |
| CT-005 | Registro como ADMIN | Confirmar que agora é bloqueado |
| CT-006 | Acesso sem token | Garantir que proteção de rotas continua |
| CT-007 | MEMBER acessa rota ADMIN | Garantir que permissões continuam corretas |

**Resultado esperado após correção:**
- CT-005 deve retornar 400 ou ignorar o campo role
- Todos os outros devem continuar passando normalmente

---

## Regressão 2 — BUG-001 (Reset de senha)

**Cenário simulado:** O dev corrigiu a função resetPassword()
incluindo o campo password na query de atualização do banco.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-001 | Login válido | Garantir que login normal não foi afetado |
| CT-003 | Login senha errada | Garantir que bloqueio continua funcionando |
| CT-008 | Reset de senha | Confirmar que nova senha agora funciona |

**Resultado esperado após correção:**
- CT-008: login com nova senha deve funcionar
- CT-008: login com senha antiga deve ser bloqueado
- CT-001 e CT-003 devem continuar passando normalmente

---

## Regressão 3 — BUG-004 (Upload inválido retorna 500)

**Cenário simulado:** O dev corrigiu o handler de erros do
upload para retornar 400 em vez de 500.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-010 | Check-in com foto válida | Garantir que upload normal continua |
| CT-011 | Check-in sem foto | Garantir que validação continua |
| CT-012 | Check-in com PDF | Confirmar que agora retorna 400 |
| CT-013 | Check-in arquivo vazio | Confirmar que agora retorna 400 |

**Resultado esperado após correção:**
- CT-012 e CT-013 devem retornar 400
- CT-010 e CT-011 devem continuar passando normalmente

---

## Regressão 4 — BUG-005 (Múltiplos check-ins no mesmo dia)

**Cenário simulado:** O dev adicionou validação que impede
mais de um check-in por dia por usuário.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-010 | Check-in com foto válida | Garantir que primeiro check-in do dia funciona |
| CT-014 | Dois check-ins no mesmo dia | Confirmar que segundo é bloqueado |
| CT-017 | Pontos somados após check-in | Garantir que pontos ainda somam corretamente |

**Resultado esperado após correção:**
- CT-014: segundo check-in deve retornar 400
- CT-010 e CT-017 devem continuar passando normalmente

---

## Regressão 5 — BUG-006 (Delete não subtrai pontos)

**Cenário simulado:** O dev corrigiu a função delete para
atualizar a tabela de pontos ao remover um check-in.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-017 | Pontos somados após check-in | Garantir que soma continua correta |
| CT-018 | Ranking ordenado | Garantir que ordenação continua correta |
| CT-019 | Pontos subtraídos ao deletar | Confirmar que agora subtrai corretamente |
| CT-020 | Admin não aparece no ranking | Garantir que ranking continua sem admin |

**Resultado esperado após correção:**
- CT-019: pontos devem diminuir após delete
- Todos os outros devem continuar passando normalmente

---

## Regressão 6 — BUG-002 (Admin no ranking)

**Cenário simulado:** O dev adicionou filtro por role no
endpoint GET /api/rankings.

**Testes a repetir:**

| CT | Descrição | Por que repetir |
|---|---|---|
| CT-017 | Pontos somados após check-in | Garantir que MEMBER ainda pontua |
| CT-018 | Ranking ordenado | Garantir que ordenação continua correta |
| CT-020 | Admin não aparece no ranking | Confirmar que admin foi removido |

**Resultado esperado após correção:**
- CT-020: admin não deve mais aparecer no ranking
- CT-017 e CT-018 devem continuar passando normalmente
