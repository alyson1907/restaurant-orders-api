# Restaurant Orders API - Versão 2

Esta é uma API backend desenvolvida para fins educativos utilizando **Node.js** e **TypeScript**.

Seu principal objetivo é simular o gerenciamento pedidos de um restaurante, oferecendo uma experiência de acompanhamento de status em tempo real com **WebSockets**.

A API permite que clientes realizem seus pedidos nos restaurantes e acompanhem o status da entrega, enquanto os restaurantes têm controle sobre a atualização dos status dos pedidos.

## Sumário

- [Instalação e Execução](#instalação-e-execução)
- [Funcionalidades](#funcionalidades)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Uso](#uso)

## Instalação e Execução

1. Clone este repositório:
   ```bash
   git clone https://github.com/seu-usuario/restaurant-orders-api.git
   ```
2. Navegue até o diretório do projeto:
   ```bash
   cd restaurant-orders-api
   ```
3. Instale as dependências:
   ```bash
   npm install
   ```
4. Configure as variáveis de ambiente:

   - Crie um arquivo `.env` com as variáveis necessárias, como chave secreta JWT, configurações do banco de dados, etc.

5. Inicie o servidor:
   ```bash
   npm start
   ```

## Funcionalidades

### Endpoints da API

- **POST `/api/client/restaurant/:id/order`**

  - Cria um novo pedido para o restaurante com o `id` especificado.
  - **Requisição:** informações do pedido no corpo (JSON).
  - **Resposta:** detalhes do pedido criado.

- **WebSocket `/api/client/order/:id`**

  - Acompanha o status de um pedido em tempo real através de WebSockets.
  - Status possíveis:
    - `CREATED`: Pedido criado.
    - `IN_PROGRESS`: Pedido em preparo.
    - `OUT_FOR_DELIVERY`: Pedido saiu para entrega.
    - `FINISHED`: Pedido entregue.
    - `CANCELED`: Pedido cancelado.

- **POST `/api/restaurant/login`**

  - Autenticação de usuário para o restaurante.
  - **Requisição:** email e senha no corpo (JSON).
  - **Resposta:** retorna um token JWT nos cookies para autenticação em endpoints protegidos.

- **PATCH `/api/restaurant/order/:id`**
  - Atualiza o status de um pedido específico.
  - **Requisição:** novo status no corpo (JSON).
  - **Regra de negócio:** utiliza uma máquina de estados para controlar a transição de status. Por exemplo:
    - Um pedido com status `IN_PROGRESS` não pode ser atualizado para `FINISHED` sem antes passar pelo status `OUT_FOR_DELIVERY`.
  - **Resposta:** retorna erro 400 caso o status seja inválido, ou 200 caso a operação tenha sido feita corretamente
  - **Permissão:** verifica se o usuário autenticado tem permissão para atualizar o pedido do restaurante especificado.

## Tecnologias Utilizadas

- **Node.js**
- **TypeScript**
- **Express** (criação de rotas e controle de requisições HTTP)
- **WebSockets** (acompanhamento de pedidos em tempo real)
- **JWT** (autenticação de usuários)

## Uso

### Exemplos de Requisição

#### Criar Pedido

```http
POST /api/client/restaurant/1/order
Content-Type: application/json
Authorization: Bearer jwt_token

{
  "items": [
    { "name": "Pizza de Calabresa", "quantity": 1 },
    { "name": "Refrigerante Lata", "quantity": 2 }
  ]
}
```
