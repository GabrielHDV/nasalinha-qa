# 🛡️ NaSalinha QA Audit

<p align="center">
  <table>
    <tr>
      <td><img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge"/></td>
      <td><img src="https://img.shields.io/badge/QA-Manual%20Testing-blue?style=for-the-badge"/></td>
      <td><img src="https://img.shields.io/badge/API-Insomnia-orange?style=for-the-badge"/></td>
      <td><img src="https://img.shields.io/badge/Docker-Environment-2496ED?style=for-the-badge&logo=docker&logoColor=white"/></td>
      <td><img src="https://img.shields.io/badge/Tests-18-informational?style=for-the-badge"/></td>
      <td><img src="https://img.shields.io/badge/PASS-10-success?style=for-the-badge"/></td>
      <td><img src="https://img.shields.io/badge/FAIL-8-red?style=for-the-badge"/></td>
    </tr>
  </table>
</p>

<p align="center">
Auditoria completa de qualidade do sistema <strong>NaSalinha</strong>, envolvendo testes funcionais, testes de API, segurança, regressão e validação de regras de negócio.
</p>

---

# 📌 Sobre o Projeto

Este repositório contém a documentação completa da auditoria de QA realizada no sistema **NaSalinha**, um sistema de check-in gamificado desenvolvido para **Comp Júnior (UFLA)**.

O objetivo do projeto foi validar o comportamento das funcionalidades principais do sistema, identificar falhas críticas e documentar os problemas encontrados através de:

- Casos de teste
- Bug Reports
- Evidências de execução
- Testes funcionais
- Testes de API
- Testes de regressão
- Relatório final de execução

---

# 🎯 Áreas Core Auditadas

## 🔐 Autenticação JWT
- Login
- Controle de acesso
- Proteção de rotas
- Permissões por role
- Segurança de autenticação

## 📸 Check-in por Foto
- Upload de imagens
- Validação de arquivos
- Tratamento de erros
- Regras de check-in

## 🏆 Sistema de Pontos e Ranking
- Soma de pontuação
- Persistência no banco
- Ordenação do ranking
- Consistência após exclusões

---

# 🧪 Tipos de Teste Realizados

✅ Testes Funcionais  
✅ Testes de API  
✅ Testes Positivos  
✅ Testes Negativos  
✅ Testes de Segurança  
✅ Testes de Regressão  
✅ Validação de Status Codes  
✅ Validação de Regras de Negócio  

---

# 🛠️ Ferramentas Utilizadas

| Ferramenta | Finalidade |
|---|---|
| Insomnia | Testes de API |
| Docker | Ambiente local e conteinerização|
| Node.js | Backend da aplicação |
| PostgreSQL | Banco de dados |
| GitHub | Versionamento e documentação |
| Markdown | Documentação técnica |

---

# 📊 Resultado da Execução

| Métrica | Valor |
|---|---|
| Casos Executados | 18 |
| PASS | 10 |
| FAIL | 8 |
| Status Final | Reprovado (Aguardando correções) |

---

# 🐞 Principais Bugs Encontrados

## 🚨 Segurança
- Escalada de privilégio via role ADMIN
- Controle inadequado de permissões

## ⚠️ Backend / API
- Upload de PDF gerando erro 500
- Arquivo vazio causando falha interna
- Reset de senha sem persistência no banco

## 📉 Regra de Negócio
- Múltiplos check-ins permitidos no mesmo dia
- Pontos não subtraídos após exclusão
- ADMIN aparecendo no ranking

---

# 📂 Estrutura do Repositório

```bash
.
├── README.md
├── docs
│   ├── casos-de-teste.md
│   ├── evidencias
│   └── bug-reports
│
├── api-collection
│   └── nasalinha-insomnia.yaml
```

---

# 🚀 Como Executar o Projeto

## 1️⃣ Clonar o repositório

Clone este repositório em sua máquina local:

```bash
git clone https://github.com/GabrielHDV/nasalinha-qa.git
```

Após o download, acesse a pasta do projeto:

```bash
cd nasalinha-qa
```

---

## 2️⃣ Configurar variáveis de ambiente

Antes de executar o sistema, é necessário configurar as variáveis de ambiente utilizadas pelo backend.

As principais integrações utilizadas são:

- PostgreSQL
- JWT
- Cloudinary
- Mailtrap

Exemplo de configuração:

```env
DATABASE_URL=
JWT_SECRET=

CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=

MAILTRAP_USER=
MAILTRAP_PASS=
```

---

## 3️⃣ Subir os containers Docker

Com o Docker aberto na máquina, execute:

```bash
docker-compose up --build
```

Esse comando irá:

- Criar os containers necessários
- Instalar dependências
- Inicializar backend e banco de dados
- Preparar o ambiente local para os testes

---

## 4️⃣ Executar seed do banco de dados

Após os containers estarem rodando, execute o seed para popular o banco com dados iniciais:

```bash
docker-compose exec backend npx prisma db seed
```

Esse passo é importante para disponibilizar usuários e dados necessários para os testes documentados.

---

## 5️⃣ Acessar a aplicação

Após a inicialização do ambiente, a aplicação poderá ser acessada através dos seguintes endereços:

### Frontend

```bash
http://localhost:3000
```

### API

```bash
http://localhost:5001
```

---

## 6️⃣ Importar collection no Insomnia

Para executar os testes de API:

1. Abrir o Insomnia
2. Clicar em **Import**
3. Selecionar a collection localizada em:

```bash
/api-collection
```

4. Verificar se a Base URL está apontando para:

```bash
http://localhost:5001
```

5. Executar as requisições conforme os casos de teste documentados

---

## 7️⃣ Executar os casos de teste

Os casos de teste estão documentados na pasta:

```bash
/docs
```

Os testes foram organizados nas seguintes áreas:

- Autenticação JWT
- Check-in por Foto
- Sistema de Pontos e Ranking

Incluindo:
- Cenários positivos
- Cenários negativos
- Testes funcionais
- Testes de API
- Testes de regressão

---

## 8️⃣ Consultar Bug Reports e Evidências

Os Bug Reports documentados estão localizados em:

```bash
/docs/bug-reports
```

As evidências de execução podem ser encontradas em:

```bash
/docs/evidencias
```

Incluindo:
- Prints de erros
- Capturas do Insomnia
- Logs da aplicação
- Evidências de comportamento inconsistente

---

## 9️⃣ Validar relatório final

O relatório consolidado da auditoria contém:

- Casos executados
- Resultados PASS/FAIL
- Bugs encontrados
- Regressões documentadas
- Validação das funcionalidades core do sistema


# 📁 Documentações

| Documento | Descrição |
|---|---|
| Casos de Teste | Planejamento e execução dos cenários |
| Bug Reports | Documentação detalhada das falhas |
| Evidências | Prints e logs |
| API Collection | Collection exportada do Insomnia |

---

# 🔍 Estratégia de QA

A auditoria foi realizada seguindo o fluxo:

1. Planejamento dos cenários
2. Execução manual
3. Testes de API
4. Registro de falhas
5. Análise dos problemas encontrados
6. Testes de regressão
7. Consolidação dos resultados

---

# 📸 Evidências

As evidências dos testes estão localizadas em:

```bash
/docs/evidencias
```

Incluindo:
- Prints de erros
- Capturas do Insomnia
- Logs da aplicação
- Evidências de comportamento inconsistente

---

# ⭐ Considerações Finais

A auditoria identificou falhas importantes relacionadas a:
- segurança
- validação de entrada
- consistência de regras de negócio
- persistência de dados

Os testes executados permitiram mapear comportamentos críticos que impactam diretamente a estabilidade e confiabilidade do sistema.
