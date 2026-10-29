# Arquitetura

## Componentes

```text
Navegador
   |
   v
Angular SSR (web/doce-lar)
   |  REST/JSON + Bearer JWT
   v
FastAPI (server)
   |
   v
SQLModel / SQLAlchemy
   |
   v
Banco relacional
```

## Frontend

O frontend usa Angular standalone components, Angular Router e SSR com Express.

Rotas da aplicação:

| Rota | Tela |
| --- | --- |
| `/` | Vitrine, carrinho, checkout e histórico |
| `/login` | Entrada de usuário |
| `/cadastro` | Criação de conta |

Responsabilidades principais:

- `AuthService`: login, cadastro, logout e sessão no `localStorage`.
- `CartService`: itens, quantidades, total e persistência do carrinho no `localStorage`.
- `Welcome`: carregamento de produtos, checkout e histórico de pedidos.
- `environment*.ts`: endereço da API por ambiente.

O token retornado pelo backend é enviado no cabeçalho:

```http
Authorization: Bearer <token>
```

## Backend

O backend é uma aplicação FastAPI criada em `server/main.py`.

- `main.py`: inicialização da aplicação, CORS, criação das tabelas e endpoint de saúde.
- `routes.py`: autenticação, produtos e pedidos.
- `database/connection.py`: carregamento do `.env`, criação do engine e sessões.
- `database/models/models.py`: modelos SQLModel.

As senhas são armazenadas com hash Argon2 por meio do `pwdlib`. Os tokens de acesso são JWT com algoritmo HS256 e expiração de 15 minutos.

## Modelo de dados

| Entidade | Finalidade |
| --- | --- |
| `User` | Conta e credenciais do cliente |
| `Product` | Cupcake, preço e estoque |
| `UserCart` | Itens associados ao carrinho de um usuário |
| `Order` | Pedido e total |
| `OrderProduct` | Produtos e quantidades de um pedido |
| `Payment` | Forma, valor e status do pagamento |
| `Shipment` | Estrutura para dados de entrega |

Ao finalizar um pedido, a API valida os produtos e o estoque, calcula o total no servidor, reduz as quantidades, registra o pedido e cria um pagamento com status `paid`. O pedido exige autenticação.

## Integrações e deploy

- O frontend tem configuração de produção para o backend hospedado no Railway.
- `web/doce-lar/vercel.json` e a configuração SSR preparam o frontend para Vercel.
- `server/railway.json` inicia o backend com Uvicorn e usa `/health` como health check.

## Observações de manutenção

- O esquema é criado automaticamente na inicialização; não há migrações documentadas no momento.
- O modelo contém estruturas de carrinho e entrega, mas os endpoints públicos atuais concentram-se em autenticação, produtos e pedidos.
- O estoque deve ser cadastrado diretamente no banco até que um fluxo administrativo seja disponibilizado.
