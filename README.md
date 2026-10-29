# Doce Lar

Documentação do **Doce Lar**, uma loja virtual de cupcakes gourmet desenvolvida como projeto acadêmico.

O projeto é dividido em:

- **Frontend:** Angular 21 com renderização no servidor (SSR), localizado em [`web/doce-lar`](https://github.com/Lucas-Verdi/doce-lar/tree/main/web/doce-lar).
- **Backend:** FastAPI com SQLModel, autenticação JWT e persistência em banco relacional, localizado em [`server`](https://github.com/Lucas-Verdi/doce-lar/tree/main/server).
- **Deploy:** frontend preparado para Vercel e backend configurado para Railway.

## Documentação

- [Guia de desenvolvimento](docs/desenvolvimento.md)
- [Arquitetura](docs/arquitetura.md)
- [API](docs/api.md)

## Visão geral

O usuário pode:

1. Acessar a vitrine de cupcakes.
2. Criar uma conta ou entrar com e-mail e senha.
3. Adicionar produtos ao carrinho.
4. Escolher Pix ou cartão de crédito.
5. Finalizar pedidos autenticados.
6. Consultar o histórico de pedidos.

O carrinho e a sessão são mantidos no `localStorage` do navegador. Produtos, usuários e pedidos são armazenados pelo backend.

## URLs dos ambientes

| Ambiente | Frontend | Backend |
| --- | --- | --- |
| Desenvolvimento | `http://localhost:4200` | `http://localhost:8000` |
| Produção | Deploy configurado na Vercel | `https://doce-lar-production.up.railway.app` |

## Licença

Este repositório contém a documentação do projeto acadêmico Doce Lar.
