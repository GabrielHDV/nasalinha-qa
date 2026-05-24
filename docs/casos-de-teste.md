# Relatório de Execução de Testes — NaSalinha QA

## Resumo Geral

- Total de casos executados: 18
- Casos aprovados (PASS): 10
- Casos reprovados (FAIL): 8

## Áreas cobertas

- Autenticação JWT
- Controle de acesso por role
- Upload e validação de arquivos
- Check-in por foto
- Sistema de pontos
- Ranking de usuários
- Regressão funcional

---

## Ambiente de Testes

**Base URL:**  
http://localhost:5001

---

# Área 1 — Autenticação JWT

### CT-001 — Login com credenciais corretas
**Tipo:** API | Positivo  
**Endpoint:** POST /api/auth/login

**Body:**
```json
{
  "email": "lucas@example.com",
  "password": "senha123"
}
```

**Esperado:** Status 200, retorna accessToken e refreshToken.  
**Obtido:** Status 200 — retornou accessToken e refreshToken.  
**Status:** PASS

---

### CT-002 — Login com e-mail em letras maiúsculas
**Tipo:** API | Negativo

**Observação:** Verificar consistência de tratamento de case no e-mail durante autenticação.

**Endpoint:** POST /api/auth/login

**Body:**
```json
{
  "email": "LUCAS@EXAMPLE.COM",
  "password": "senha123"
}
```

**Esperado:** Status 200 (se normalizar) ou 401 com mensagem clara.  
**Obtido:** Status 401 — sistema não normaliza e-mail.  
**Status:** FAIL

---

### CT-003 — Login com senha errada
**Tipo:** API | Negativo

**Endpoint:** POST /api/auth/login

**Body:**
```json
{
  "email": "lucas@example.com",
  "password": "senhaerrada"
}
```

**Esperado:** Status 401.  
**Obtido:** Status 401 — credenciais inválidas.  
**Status:** PASS

---

### CT-004 — Login com espaço acidental na senha
**Tipo:** API | Negativo

**Observação:** Erro comum quando o usuário copia a senha de algum lugar e vem com espaço no início.

**Endpoint:** POST /api/auth/login

**Body:**
```json
{
  "email": "lucas@example.com",
  "password": " senha123"
}
```

**Esperado:** Status 401. O sistema não deve ignorar o espaço.  
**Obtido:** Status 401 — espaço não foi ignorado.  
**Status:** PASS

---

### CT-005 — Registro enviando role ADMIN no body
**Tipo:** API | Negativo — Segurança

**Observação:** Verificar se o campo role enviado pelo cliente é aceito ou ignorado pelo servidor.

**Endpoint:** POST /api/auth/register

**Body:**
```json
{
  "name": "Invasor",
  "email": "invasor@teste.com",
  "password": "senha123",
  "role": "ADMIN"
}
```

**Esperado:** Usuário criado como MEMBER. Campo role deve ser ignorado.  
**Obtido:** Status 201 — usuário criado como ADMIN.  
**Status:** FAIL

**Severidade:** CRÍTICO

---

### CT-006 — Acessar rota protegida sem token
**Tipo:** API | Negativo

**Endpoint:** GET /api/users/me

**Headers:** Sem Authorization

**Esperado:** Status 401.  
**Obtido:** Status 401 — token não fornecido.  
**Status:** PASS

---

### CT-007 — Acessar rota de ADMIN com token de MEMBER
**Tipo:** API | Negativo — Segurança

**Observação:** Token válido mas de usuário sem permissão.

**Endpoint:** GET /api/users

**Headers:** Authorization: Bearer {token do Lucas}

**Esperado:** Status 403.  
**Obtido:** Status 403 — acesso corretamente bloqueado.  
**Status:** PASS

---

### CT-008 — Resetar senha e tentar logar com a nova
**Tipo:** Funcional (Interface) | Negativo

**Observação:** Suspeita de falha na persistência da nova senha no banco.

**Passos:**
1. Clicar em "Esqueci minha senha"
2. Informar lucas@example.com
3. Clicar no link recebido no Mailtrap
4. Digitar nova senha: novaSenha456
5. Tentar login com novaSenha456
6. Tentar login com senha123

**Esperado:** Login com nova senha funciona e senha antiga deixa de funcionar.  
**Obtido:** Status 200, porém a nova senha não foi salva no banco.  
**Status:** FAIL

**Severidade:** CRÍTICO

---

### CT-009 — Regressão após correção do CT-005
**Tipo:** Regressão

**Cenário:** Bug do registro como ADMIN foi corrigido.

**O que repetir:**
- Reexecutar CT-001 para validar se o login continua funcionando normalmente
- Reexecutar CT-005 para confirmar que o role ADMIN agora é ignorado
- Validar se usuários MEMBER continuam conseguindo se cadastrar normalmente
- Validar geração correta do JWT após alteração da regra de acesso

---

# Área 2 — Check-in por Foto

### CT-010 — Check-in com foto válida
**Tipo:** API | Positivo

**Endpoint:** POST /api/checkins

**Headers:** Authorization: Bearer {token MEMBER}

**Body:** multipart/form-data com imagem JPG ou PNG

**Esperado:** Status 201 com dados do check-in.  
**Obtido:** Status 201 — check-in criado com sucesso.  
**Status:** PASS

---

### CT-011 — Check-in sem enviar nenhuma foto
**Tipo:** API | Negativo

**Endpoint:** POST /api/checkins

**Esperado:** Status 400.  
**Obtido:** Status 400 — foto é obrigatória.  
**Status:** PASS

---

### CT-012 — Check-in enviando PDF no lugar de imagem
**Tipo:** API | Negativo

**Observação:** Verificar validação de tipo de arquivo.

**Endpoint:** POST /api/checkins

**Esperado:** Status 400.  
**Obtido:** Status 500 — sistema lançou erro interno ao invés de validar o arquivo.  
**Status:** FAIL

**Severidade:** MÉDIO

---

### CT-013 — Check-in com arquivo vazio (0 bytes)
**Tipo:** API | Negativo

**Observação:** Arquivo corrompido ou inválido.

**Endpoint:** POST /api/checkins

**Esperado:** Status 400.  
**Obtido:** Status 500 — arquivo vazio não foi tratado corretamente.  
**Status:** FAIL

**Severidade:** MÉDIO

---

### CT-014 — Dois check-ins no mesmo dia pelo mesmo usuário
**Tipo:** API | Negativo

**Observação:** Possível abuso de pontuação.

**Endpoint:** POST /api/checkins

**Esperado:** Segundo check-in bloqueado.  
**Obtido:** Status 201 — segundo check-in aceito sem limite diário.  
**Status:** FAIL

**Severidade:** ALTO

---

### CT-015 — MEMBER tentar deletar check-in de outro usuário
**Tipo:** API | Negativo — Segurança

**Endpoint:** DELETE /api/checkins/{id}

**Esperado:** Status 403.  
**Obtido:** Status 403 — sem permissão.  
**Status:** PASS

---

### CT-016 — Regressão após correção do CT-012
**Tipo:** Regressão

**Cenário:** Correção da validação de arquivos.

**O que repetir:**
- Reexecutar CT-010 para garantir que uploads válidos continuam funcionando
- Reexecutar CT-012 para validar bloqueio de arquivos PDF
- Reexecutar CT-013 para validar tratamento correto de arquivos vazios
- Validar se a soma de pontos continua funcionando após check-in válido

---

# Área 3 — Sistema de Pontos e Ranking

### CT-017 — Pontos somados corretamente após check-in
**Tipo:** API | Positivo

**Passos:**
1. Consultar ranking
2. Realizar check-in válido
3. Consultar ranking novamente

**Esperado:** Pontos aumentam em 10.  
**Obtido:** Pontos aumentaram de 100 para 110.  
**Status:** PASS

---

### CT-018 — Ranking ordenado do maior para o menor
**Tipo:** API | Positivo

**Endpoint:** GET /api/rankings

**Esperado:** Ranking ordenado corretamente.  
**Obtido:** Status 200 — ranking retornado em ordem correta.  
**Status:** PASS

---

### CT-019 — Pontos subtraídos quando ADMIN deleta check-in
**Tipo:** API | Negativo

**Observação:** Consistência do ranking após exclusão.

**Passos:**
1. Consultar ranking
2. Deletar check-in
3. Consultar ranking novamente

**Esperado:** Pontos reduzidos corretamente.  
**Obtido:** Pontos não foram subtraídos após exclusão do check-in.  
**Status:** FAIL

**Severidade:** ALTO

---

### CT-020 — ADMIN não aparece no ranking
**Tipo:** Funcional (Interface) | Positivo

**Observação:** Administradores não deveriam competir com membros.

**Passos:**
1. Login como ADMIN
2. Fazer check-in
3. Acessar ranking

**Esperado:** ADMIN não aparece no ranking.  
**Obtido:** Admin apareceu no ranking com 10 pontos.  
**Status:** FAIL

**Severidade:** ALTO

---

### CT-021 — Regressão após correção do CT-019
**Tipo:** Regressão

**Cenário:** Correção da inconsistência de pontos após exclusão.

**O que repetir:**
- Reexecutar CT-017 para validar se os pontos continuam sendo somados corretamente
- Reexecutar CT-019 para confirmar subtração correta após exclusão
- Reexecutar CT-020 para garantir que ADMIN continua fora do ranking
- Validar ordenação correta do ranking após alterações
