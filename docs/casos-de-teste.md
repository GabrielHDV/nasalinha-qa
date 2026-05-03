## Área 1 — Autenticação JWT

### CT-001 — Login com credenciais corretas
**Tipo:** API | Positivo  
**Endpoint:** POST http://localhost:5001/api/auth/login  
**Body:**
```json
{
  "email": "lucas@example.com",
  "password": "senha123"
}
```
**Esperado:** Status 200, retorna accessToken e refreshToken.  
**Obtido:**  
**Status:**  

---

### CT-002 — Login com e-mail em letras maiúsculas
**Tipo:** API | Negativo  
**Observação:** Verificar se o sistema diferencia "lucas@example.com" de "LUCAS@EXAMPLE.COM".  
**Endpoint:** POST http://localhost:5001/api/auth/login  
**Body:**
```json
{
  "email": "LUCAS@EXAMPLE.COM",
  "password": "senha123"
}
```
**Esperado:** Status 200 (se normalizar) ou 401 com mensagem clara.  
**Obtido:**  
**Status:**  

---

### CT-003 — Login com senha errada
**Tipo:** API | Negativo  
**Endpoint:** POST http://localhost:5001/api/auth/login  
**Body:**
```json
{
  "email": "lucas@example.com",
  "password": "senhaerrada"
}
```
**Esperado:** Status 401.  
**Obtido:**  
**Status:**  

---

### CT-004 — Login com espaço acidental na senha
**Tipo:** API | Negativo  
**Observação:** Erro comum quando o usuário copia a senha de algum lugar e vem com espaço no início.  
**Endpoint:** POST http://localhost:5001/api/auth/login  
**Body:**
```json
{
  "email": "lucas@example.com",
  "password": " senha123"
}
```
**Esperado:** Status 401. O sistema não deve ignorar o espaço.  
**Obtido:**  
**Status:**  

---

### CT-005 — Registro enviando role ADMIN no body
**Tipo:** API | Negativo — Segurança  
**Observação:** Verificar se o campo role enviado pelo cliente é aceito ou ignorado pelo servidor.  
**Endpoint:** POST http://localhost:5001/api/auth/register  
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
**Obtido:**  
**Status:**  

---

### CT-006 — Acessar rota protegida sem token
**Tipo:** API | Negativo  
**Endpoint:** GET http://localhost:5001/api/users/me  
**Headers:** Sem Authorization  
**Esperado:** Status 401.  
**Obtido:**  
**Status:**  

---

### CT-007 — Acessar rota de ADMIN com token de MEMBER
**Tipo:** API | Negativo — Segurança  
**Observação:** Token válido mas de usuário sem permissão. O sistema deve barrar pelo role.  
**Endpoint:** GET http://localhost:5001/api/users  
**Headers:** Authorization: Bearer {token do lucas}  
**Esperado:** Status 403.  
**Obtido:**  
**Status:**  

---

### CT-008 — Resetar senha e tentar logar com a nova
**Tipo:** Funcional (Interface) | Negativo  
**Observação:** Suspeita de bug — a função de reset pode não estar salvando a nova senha no banco.  
**Passos:**
1. Clicar em "Esqueci minha senha"
2. Informar lucas@example.com
3. Clicar no link que chega no Mailtrap
4. Digitar nova senha: novaSenha456
5. Tentar logar com novaSenha456
6. Tentar logar com senha123 (antiga)

**Esperado:** Login com novaSenha456 funciona. Senha antiga não funciona mais.  
**Obtido:**  
**Status:**  

---

### CT-009 — Regressão após correção do CT-005
**Tipo:** Regressão  
**Cenário:** Bug do registro como ADMIN foi corrigido.  
**O que repetir:**
- CT-001: login normal ainda funciona?
- CT-005: registro como ADMIN agora é bloqueado?
- Cadastro normal como MEMBER ainda funciona?
- Token gerado ainda autentica rotas protegidas?

---

## Área 2 — Check-in por Foto

### CT-010 — Check-in com foto válida
**Tipo:** API | Positivo  
**Endpoint:** POST http://localhost:5001/api/checkins  
**Headers:** Authorization: Bearer {token de MEMBER}  
**Body:** multipart/form-data — campo "photo" com imagem JPG ou PNG  
**Esperado:** Status 201, retorna dados do check-in com URL da foto e pontos somados.  
**Obtido:**  
**Status:**  

---

### CT-011 — Check-in sem enviar nenhuma foto
**Tipo:** API | Negativo  
**Endpoint:** POST http://localhost:5001/api/checkins  
**Headers:** Authorization: Bearer {token de MEMBER}  
**Body:** multipart/form-data vazio  
**Esperado:** Status 400.  
**Obtido:**  
**Status:**  

---

### CT-012 — Check-in enviando PDF no lugar de imagem
**Tipo:** API | Negativo  
**Observação:** Verificar se o sistema valida o tipo do arquivo enviado.  
**Endpoint:** POST http://localhost:5001/api/checkins  
**Headers:** Authorization: Bearer {token de MEMBER}  
**Body:** multipart/form-data — campo "photo" com arquivo .pdf  
**Esperado:** Status 400. Sistema rejeita arquivos que não são imagem.  
**Obtido:**  
**Status:**  

---

### CT-013 — Check-in com arquivo vazio (0 bytes)
**Tipo:** API | Negativo  
**Observação:** Arquivo corrompido ou gerado errado. Criar com: touch foto_vazia.jpg  
**Endpoint:** POST http://localhost:5001/api/checkins  
**Headers:** Authorization: Bearer {token de MEMBER}  
**Body:** multipart/form-data — campo "photo" com foto_vazia.jpg  
**Esperado:** Status 400.  
**Obtido:**  
**Status:**  

---

### CT-014 — Dois check-ins no mesmo dia pelo mesmo usuário
**Tipo:** API | Negativo  
**Observação:** Se não houver bloqueio, um usuário pode inflar os pontos fazendo vários check-ins no mesmo dia.  
**Endpoint:** POST /api/checkins (executar duas vezes seguidas com o mesmo token)  
**Esperado:** Segundo check-in bloqueado com mensagem de limite diário.  
**Obtido:**  
**Status:**  

---

### CT-015 — MEMBER tentar deletar check-in de outro usuário
**Tipo:** API | Negativo — Segurança  
**Endpoint:** DELETE http://localhost:5001/api/checkins/{id do check-in da Maria}  
**Headers:** Authorization: Bearer {token do Lucas}  
**Esperado:** Status 403.  
**Obtido:**  
**Status:**  

---

### CT-016 — Regressão após correção do CT-012
**Tipo:** Regressão  
**Cenário:** Validação de tipo de arquivo foi corrigida.  
**O que repetir:**
- CT-010: check-in com JPG ainda funciona?
- CT-012: PDF agora é bloqueado?
- CT-013: arquivo vazio ainda é bloqueado?
- Pontos ainda somam após check-in válido?

---

## Área 3 — Sistema de Pontos e Ranking

### CT-017 — Pontos somados corretamente após check-in
**Tipo:** API | Positivo  
**Passos:**
1. GET /api/rankings — anotar pontos atuais
2. POST /api/checkins com foto válida
3. GET /api/rankings novamente

**Esperado:** Pontos aumentaram exatamente 10.  
**Obtido:**  
**Status:**  

---

### CT-018 — Ranking ordenado do maior para o menor
**Tipo:** API | Positivo  
**Endpoint:** GET http://localhost:5001/api/rankings  
**Esperado:** Status 200. Lucas (20pts) antes de Maria (10pts), antes de Pedro (0pts).  
**Obtido:**  
**Status:**  

---

### CT-019 — Pontos subtraídos quando ADMIN deleta check-in
**Tipo:** API | Negativo  
**Observação:** Se deletar um check-in não subtrai os pontos, o ranking fica incorreto.  
**Passos:**
1. GET /api/rankings — anotar pontos do Lucas
2. DELETE /api/checkins/{id} como ADMIN
3. GET /api/rankings novamente

**Esperado:** Pontos do Lucas reduziram em 10.  
**Obtido:**  
**Status:**  

---

### CT-020 — ADMIN não aparece no ranking
**Tipo:** Funcional (Interface) | Positivo  
**Observação:** Admin não deveria competir com os membros.  
**Passos:**
1. Login como admin@nasalinha.com
2. Fazer um check-in
3. Acessar o ranking

**Esperado:** Admin não aparece na lista.  
**Obtido:**  
**Status:**  

---

### CT-021 — Regressão após correção do CT-019
**Tipo:** Regressão  
**Cenário:** Bug de pontos não subtraídos ao deletar check-in foi corrigido.  
**O que repetir:**
- CT-017: pontos ainda somam ao fazer check-in?
- CT-019: pontos agora são subtraídos ao deletar?
- CT-020: admin ainda não aparece no ranking?
- Ranking ainda ordena corretamente após as alterações?
