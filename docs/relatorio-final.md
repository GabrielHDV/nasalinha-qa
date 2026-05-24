# Relatório de Testes Final — NaSalinha QA

## Informações Gerais

**Projeto:** NaSalinha — Sistema de Check-in Gamificado  
**Repositório auditado:** github.com/Gustavohmmagalhaes/nasalinha-qa-challenge  
**Período de testes:** Semanas 1 a 6  
**Status Final:** Reprovado — Aguardando correções  

---

## Métricas de Execução

| Métrica | Valor |
|---|---|
| Total de Casos Planejados | 21 |
| Total de Casos Executados | 18 |
| Casos que Passaram (Pass) | 10 |
| Casos que Falharam (Fail) | 8 |
| Casos Descritivos (Regressão) | 3 |

---

## Distribuição de Bugs por Severidade

| Severidade | Quantidade | Bugs |
|---|---|---|
| Crítica | 2 | BUG-001, BUG-003 |
| Alta | 3 | BUG-002, BUG-005, BUG-006 |
| Média | 1 | BUG-004 |
| Baixa | 0 | — |

---

## Resumo por Área

| Área | Casos Executados | Pass | Fail | Bugs |
|---|---|---|---|---|
| Autenticação JWT | 8 | 4 | 4 | BUG-001, BUG-003 |
| Check-in por Foto | 6 | 3 | 3 | BUG-004, BUG-005 |
| Sistema de Pontos | 4 | 2 | 2 | BUG-002, BUG-006 |

---

## Bugs Encontrados

| ID | Descrição | Severidade | Área |
|---|---|---|---|
| BUG-001 | Reset de senha não salva no banco | Crítica | JWT |
| BUG-002 | Admin aparece no ranking | Alta | Pontos |
| BUG-003 | Registro aceita role ADMIN | Crítica | JWT |
| BUG-004 | Upload inválido retorna 500 | Média | Check-in |
| BUG-005 | Múltiplos check-ins no mesmo dia | Alta | Check-in |
| BUG-006 | Delete não subtrai pontos | Alta | Pontos |

---

## Conclusão

A auditoria identificou 6 bugs distribuídos nas 3 áreas core
do sistema. Dois bugs de severidade Crítica foram encontrados
na área de autenticação, um deles permite escalada de
privilégios (qualquer pessoa pode se registrar como ADMIN)
e outro impede a recuperação de acesso à conta via reset
de senha.

O sistema não está apto para deploy. As
correções prioritárias são BUG-001 e BUG-003 por
comprometerem diretamente a segurança da aplicação.
