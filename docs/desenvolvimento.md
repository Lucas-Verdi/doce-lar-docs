# Desenvolvimento

## Pré-requisitos

- Git
- Node.js e npm
- Python 3.12 ou superior
- `uv`
- Um banco compatível com SQLAlchemy. O projeto está configurado para MySQL.

Os comandos abaixo usam PowerShell no Windows.

## Clonar o projeto

```powershell
git clone https://github.com/Lucas-Verdi/doce-lar.git
cd doce-lar
```

O repositório de código e o repositório de documentação são separados. Clone este repositório também se quiser trabalhar na documentação.

## Configurar o backend

```powershell
cd server
uv sync
```

Crie `server/.env` com as variáveis:

```dotenv
DATABASE_URL=mysql://usuario:senha@host:3306/nome_do_banco
JWT_SECRET=uma-chave-secreta-longa
```

O backend converte automaticamente o prefixo `mysql://` para `mysql+pymysql://`. Não use valores reais de produção em arquivos versionados.

Inicie a API:

```powershell
uv run fastapi dev
```

A API ficará disponível em `http://localhost:8000`. A documentação interativa do FastAPI fica em:

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

O endpoint `GET /health` verifica a conexão com o banco. O backend cria as tabelas registradas no SQLModel ao iniciar, caso a conexão esteja configurada.

## Configurar o frontend

Em outro terminal:

```powershell
cd web\doce-lar
npm install
npm start
```

Acesse `http://localhost:4200`.

No desenvolvimento, o Angular substitui o ambiente de produção por [`environment.development.ts`](https://github.com/Lucas-Verdi/doce-lar/blob/main/web/doce-lar/src/environments/environment.development.ts), que aponta para `http://localhost:8000`. O build de produção usa [`environment.ts`](https://github.com/Lucas-Verdi/doce-lar/blob/main/web/doce-lar/src/environments/environment.ts).

## Comandos úteis

### Frontend

```powershell
npm start       # servidor de desenvolvimento
npm run build   # build de produção
npm test        # testes unitários
```

### Backend

```powershell
uv run fastapi dev                 # desenvolvimento com recarga
uvicorn main:app --host 0.0.0.0 --port 8000
```

## Fluxo de desenvolvimento

1. Inicie o banco e configure `server/.env`.
2. Inicie o backend antes do frontend.
3. Verifique `http://localhost:8000/health`.
4. Abra a aplicação em `http://localhost:4200`.
5. Crie uma conta para testar login, carrinho e checkout.

Não há seed de produtos no backend. Para testar a vitrine, o banco precisa conter registros na tabela `product`.

## Solução de problemas

### `JWT_SECRET` não configurada

Defina `JWT_SECRET` em `server/.env` antes de iniciar a API.

### `DATABASE_URL não configurada`

Defina `DATABASE_URL` em `server/.env` e confirme se o banco está acessível.

### Frontend não acessa a API

Confirme se o backend está em execução na porta 8000 e se a configuração de desenvolvimento aponta para `http://localhost:8000`.

### Erro de CORS

O backend permite origens locais (`localhost` e `127.0.0.1`) e a URL de produção configurada no código. Use o frontend em `http://localhost:4200` durante o desenvolvimento.
