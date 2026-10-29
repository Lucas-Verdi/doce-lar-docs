# API

Base local: `http://localhost:8000`

Todos os endpoints da aplicação, exceto `/health`, usam o prefixo `/api`.

## Saúde da aplicação

### `GET /health`

Verifica se a API possui `DATABASE_URL` e consegue executar uma consulta no banco.

Resposta:

```json
{
  "status": "ok",
  "database": "connected"
}
```

Em caso de falha, retorna `503`.

## Autenticação

### `POST /api/create_account`

Cria uma conta.

Corpo:

```json
{
  "username": "cliente",
  "email": "cliente@example.com",
  "full_name": "Cliente Doce Lar",
  "password": "senha-segura"
}
```

Retorna `201` com `id`, `username`, `email` e `full_name`. A senha nunca é retornada.

### `POST /api/user_login`

Autentica pelo e-mail e senha.

Corpo:

```json
{
  "email": "cliente@example.com",
  "password": "senha-segura"
}
```

Resposta:

```json
{
  "access_token": "jwt",
  "token_type": "bearer",
  "username": "cliente",
  "full_name": "Cliente Doce Lar"
}
```

O token expira após 15 minutos. Use-o nas rotas protegidas:

```http
Authorization: Bearer jwt
```

## Produtos

### `GET /api/get_products`

Lista os produtos cadastrados.

Exemplo de resposta:

```json
[
  {
    "id": 1,
    "name": "Cupcake de chocolate",
    "description": "Cupcake gourmet",
    "price": 12.5,
    "in_stock": true,
    "quantity": 10
  }
]
```

## Pedidos

### `POST /api/orders`

Cria um pedido para o usuário autenticado.

Corpo:

```json
{
  "items": [
    {
      "product_id": 1,
      "quantity": 2
    }
  ],
  "payment_method": "pix"
}
```

`payment_method` aceita `pix` ou `credit_card`. Cada produto pode aparecer uma única vez e a quantidade deve estar entre 1 e 99.

Resposta `201`:

```json
{
  "id": 1,
  "total_amount": 25.0,
  "status": "pending",
  "payment_method": "pix"
}
```

Possíveis respostas de validação:

| Status | Situação |
| --- | --- |
| `401` | Token ausente, inválido ou usuário inexistente |
| `404` | Produto não encontrado |
| `409` | Estoque insuficiente |
| `422` | Forma de pagamento ou itens inválidos |

### `GET /api/orders`

Lista o histórico do usuário autenticado, do pedido mais recente para o mais antigo.

Resposta:

```json
[
  {
    "id": 1,
    "total_amount": 25.0,
    "payment_method": "pix",
    "items": [
      {
        "product_id": 1,
        "name": "Cupcake de chocolate",
        "price": 12.5,
        "quantity": 2,
        "subtotal": 25.0
      }
    ]
  }
]
```

## Documentação automática

Com o backend em execução, os contratos completos, schemas e testes manuais estão disponíveis em:

- `http://localhost:8000/docs`
- `http://localhost:8000/redoc`
