# Atividade 14 — Autenticação JWT e Segurança de APIs

Projeto acadêmico baseado no repositório disponibilizado na Aula 14 da disciplina de **Linguagens e Tecnologias para Desenvolvimento Web**. O objetivo é demonstrar o fluxo de autenticação *stateless* entre um front-end React e uma API Express protegida por JWT.

## Tecnologias utilizadas

- **Front-end:** React, Vite e Axios.
- **Back-end:** Node.js, Express, bcrypt, jsonwebtoken e CORS.
- **Usuário:** dados simulados (*mock*), sem banco de dados.

## Pré-requisitos

- Node.js e npm instalados.
- Dois terminais disponíveis para executar o back-end e o front-end simultaneamente.

## Executar o projeto

### 1. Back-end

Entre na pasta `backend`:

```bash
cd backend
npm install
```

Crie o arquivo local `.env` copiando o modelo `.env.example`. No Prompt de Comando do Windows:

```cmd
copy .env.example .env
```

Edite `backend/.env` e substitua o valor de `SECRET_JWT` por uma chave aleatória forte, mantendo `PORT=5000`. Para gerar uma chave de exemplo localmente, execute:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Cole a chave gerada no valor de `SECRET_JWT`. **Não publique seu arquivo `.env`.**

Inicie a API:

```bash
npm run dev
```

A API ficará disponível em `http://localhost:5000`.

### 2. Front-end

Em outro terminal, a partir da pasta principal do projeto:

```bash
cd frontend
npm install
npm run dev
```

Abra `http://localhost:5173` no navegador. O front-end está configurado para chamar a API em `http://localhost:5000/api`.

## Credenciais de demonstração

O código usa um usuário simulado para fins didáticos:

- **E-mail:** `aluno@unicatolica.edu.br`
- **Senha:** `senha123`

Os campos da tela de login começam vazios. **Essas credenciais são apenas de teste**, não destinadas a uso real.

## Funcionalidades e requisitos da Aula 14

1. CORS configurado no Express para aceitar a origem `http://localhost:5173`.
2. Validação de senha no login usando `bcrypt.compare()`.
3. Emissão de JWT com o ID do usuário por `jwt.sign()`.
4. Leitura do cabeçalho `Authorization: Bearer <token>` e verificação com `jwt.verify()` no middleware.
5. Proteção da rota `GET /api/dashboard` pelo middleware de autenticação.
6. Armazenamento do JWT no `localStorage` após login bem-sucedido.
7. Interceptor do Axios que adiciona automaticamente o token ao cabeçalho `Authorization`.
8. Logout que remove o token do `localStorage`.

## Testes manuais

1. **Login válido:** preencha as credenciais de demonstração e confirme a abertura do painel.
2. **Consulta autorizada:** no painel, clique em **Buscar Dados Sigilosos**. Deve aparecer uma mensagem de boas-vindas ao usuário 1.
3. **Rota sem token:** abra `http://localhost:5000/api/dashboard` diretamente em uma nova aba. O servidor deve responder `Token não fornecido` (HTTP 401).
4. **Persistência:** após o login, atualize a página (`F5`). A interface deve continuar exibindo o painel enquanto houver token armazenado.
5. **Logout:** clique em **Sair** e atualize a página. O formulário de login deve continuar visível.

## Observações de segurança

- O arquivo `backend/.env.example` contém **apenas valores de exemplo**. O segredo real deve ficar exclusivamente no arquivo local `backend/.env`.
- O uso de `localStorage` atende aos requisitos didáticos da atividade; aplicações de produção exigem atenção adicional a riscos de XSS e ao gerenciamento de sessões.
- O projeto não possui MongoDB nem cadastro persistente de usuários: a finalidade desta atividade é demonstrar autenticação JWT e proteção da API.
