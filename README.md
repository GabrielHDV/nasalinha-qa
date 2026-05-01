# QA NaSalinha

## Descrição
Auditoria de qualidade completa do sistema NaSalinha,
um sistema de check-in gamificado da Comp Júnior (UFLA).

## Ferramentas Utilizadas
- **Insomnia** — Testes de API e validação de endpoints
- **Docker** — Ambiente local do projeto
- **GitHub** — Gestão de documentação e bug reports

## Tipos de Testes
- Testes Funcionais (interface no navegador)
- Testes de API (endpoints via Insomnia)
- Testes de Regressão

## Como Rodar o Projeto
1. Clone o repositório NaSalinha
2. Configure o .env com Cloudinary e Mailtrap
3. Execute: docker-compose up --build
4. Execute: docker-compose exec backend npx prisma db seed
5. Acesse: http://localhost:3000

## Áreas Core Auditadas
1. Autenticação JWT
2. Check-in por Foto
3. Sistema de Pontos e Ranking
