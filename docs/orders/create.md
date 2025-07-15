# Criar Pedidos

> Envie novos pedidos para a plataforma Delivery Zen

## Visão Geral

A API de Pedidos permite que você envie pedidos diretamente para a plataforma Delivery Zen. Esta API recebe requisições POST com os dados do pedido e os processa em tempo real. O payload deve conter todos os dados necessários do pedido sem necessidade de wrapper adicional.

## Endpoint

### URL da API

```
https://api.deliveryzen.com.br/v1/orders
```

### Método

```
POST
```

### Headers

```http
Content-Type: application/json
APP_ID: YOUR_APP_ID
APP_TOKEN: YOUR_APP_TOKEN
```

## Estrutura do Payload

### Exemplo de Payload

```json
{
  "id": "123123123",
  "ticketId": "001",
  "createdAt": "2025-07-12T21:51:39.204Z",
  "type": "delivery",
  "mesa": "1",
  "timeToDelivery": "2025-07-12T21:51:39.204Z",
  "observations": "Rapido estou com fome!",
  "status": "PENDING",
  "subTotal": 21.0,
  "deliveryFee": 5.0,
  "additionalFees": 3.0,
  "discounts": 2.0,
  "total": 27.0,
  "paymentMethod": "MONEY",
  "changeAmount": 0.0,
  "paid": false,
  "customer": {
    "id": "1",
    "name": "Bruno Frank",
    "phone": "62991724016"
  },
  "deliveryAddress": {
    "street": "Rua 607",
    "number": "S/N",
    "line1": "Qd 53 Lt 20",
    "line2": "Qualquer coisa",
    "coordinates": [-16.1234, -49.5678],
    "neighborhood": "Setor Sul I",
    "city": "Uruaçu",
    "state": "GO",
    "post_code": "76400000"
  },
  "items": [
    {
      "id": 0,
      "name": "Dom Frank",
      "description": "Cebola, carne, pão queijo e bacon",
      "quantity": 1,
      "obs": "Sem cebola",
      "category": {
        "id": 1,
        "name": "Bebidas"
      },
      "addons": [
        {
          "id": 1,
          "name": "Ovo",
          "quantity": 1,
          "category": {
            "id": 1,
            "name": "Bebidas"
          }
        }
      ]
    }
  ]
}
```

## Campos do Payload

### Campos do Pedido

| Campo            | Tipo    | Descrição                                             | Exemplo                                                                    |
| ---------------- | ------- | ----------------------------------------------------- | -------------------------------------------------------------------------- |
| `id`             | string  | Código único do pedido                                | `"123123123"`                                                              |
| `ticketId`       | string  | Número apresentado ao cliente                         | `"001"`                                                                    |
| `createdAt`      | string  | Data/hora de criação (ISO 8601)                       | `"2025-07-12T21:51:39.204Z"`                                               |
| `type`           | string  | Tipo do pedido                                        | `"delivery"`, `"in_store"`, `"takeout"`                                    |
| `mesa`           | string  | Número da mesa (apenas in_store)                      | `"1"`                                                                      |
| `timeToDelivery` | string  | Tempo estimado para entrega                           | `"2025-07-12T21:51:39.204Z"`                                               |
| `observations`   | string  | Observações do pedido                                 | `"Rapido estou com fome!"`                                                 |
| `status`         | string  | Status atual do pedido                                | `"PENDING"`, `"ACCEPTED"`, `"PREPARING"`, `"DONE"`, `"CANCELLED"`          |
| `subTotal`       | number  | Total dos produtos (sem descontos, adicionais e taxa) | `21.00`                                                                    |
| `deliveryFee`    | number  | Taxa de entrega                                       | `5.00`                                                                     |
| `additionalFees` | number  | Taxas adicionais                                      | `3.00`                                                                     |
| `discounts`      | number  | Descontos aplicados                                   | `2.00`                                                                     |
| `total`          | number  | Total final                                           | `27.00`                                                                    |
| `paymentMethod`  | string  | Método de pagamento                                   | `"MONEY"`, `"CREDIT_CARD"`, `"DEBIT_CARD"`, `"PIX"`, `"TICKET"`, `"OTHER"` |
| `changeAmount`   | number  | Troco para o cliente                                  | `0.00`                                                                     |
| `paid`           | boolean | Se o pagamento foi feito antecipadamente              | `false`                                                                    |

### Tipos de Pedido

- **`delivery`**: Entrega em endereço
- **`in_store`**: Consumo no local
- **`takeout`**: Retirada no local

### Status do Pedido

- **`PENDING`**: Pedido pendente
- **`ACCEPTED`**: Pedido aceito
- **`PREPARING`**: Em preparação
- **`DONE`**: Pronto
- **`CANCELLED`**: Cancelado

### Métodos de Pagamento

- **`MONEY`**: Dinheiro
- **`CREDIT_CARD`**: Cartão de crédito
- **`DEBIT_CARD`**: Cartão de débito
- **`PIX`**: PIX
- **`TICKET`**: Vale/Boleto
- **`OTHER`**: Outros

### Estrutura do Cliente

```json
{
  "id": "1",
  "name": "Bruno Frank",
  "phone": "62991724016"
}
```

### Estrutura do Endereço de Entrega

```json
{
  "street": "Rua 607",
  "number": "S/N",
  "line1": "Qd 53 Lt 20",
  "line2": "Qualquer coisa",
  "coordinates": [-16.1234, -49.5678],
  "neighborhood": "Setor Sul I",
  "city": "Uruaçu",
  "state": "GO",
  "post_code": "76400000"
}
```

> **Nota**: Se `coordinates` for `null`, o sistema buscará as coordenadas automaticamente no Google Maps.

### Estrutura dos Itens

```json
{
  "id": 0,
  "name": "Dom Frank",
  "description": "Cebola, carne, pão queijo e bacon",
  "quantity": 1,
  "obs": "Sem cebola",
  "category": {
    "id": 1,
    "name": "Bebidas"
  },
  "addons": [
    {
      "id": 1,
      "name": "Ovo",
      "quantity": 1,
      "category": {
        "id": 1,
        "name": "Bebidas"
      }
    }
  ]
}
```

## Implementação

### Exemplo em Node.js

```javascript
const axios = require("axios");

const sendOrder = async (orderData) => {
  try {
    const response = await axios.post(
      "https://api.deliveryzen.com.br/v1/orders",
      orderData,
      {
        headers: {
          "Content-Type": "application/json",
          APP_ID: "YOUR_APP_ID",
          APP_TOKEN: "YOUR_APP_TOKEN",
        },
      }
    );

    console.log("Pedido enviado com sucesso:", response.data);
    return response.data;
  } catch (error) {
    console.error(
      "Erro ao enviar pedido:",
      error.response?.data || error.message
    );
    throw error;
  }
};

// Exemplo de uso
const orderData = {
  id: "123123123",
  ticketId: "001",
  createdAt: new Date().toISOString(),
  type: "delivery",
  status: "PENDING",
  // ... outros campos
};

sendOrder(orderData);
```

### Exemplo em Python

```python
import requests
import json
from datetime import datetime

def send_order(order_data, app_id, app_token):
    url = "https://api.deliveryzen.com.br/v1/orders"

    payload = order_data

    headers = {
        "Content-Type": "application/json",
        "APP_ID": app_id,
        "APP_TOKEN": app_token
    }

    try:
        response = requests.post(url, json=payload, headers=headers)
        response.raise_for_status()

        print("Pedido enviado com sucesso:", response.json())
        return response.json()
    except requests.exceptions.RequestException as e:
        print("Erro ao enviar pedido:", e)
        raise e

# Exemplo de uso
order_data = {
    "id": "123123123",
    "ticketId": "001",
    "createdAt": datetime.now().isoformat(),
    "type": "delivery",
    "status": "PENDING",
    # ... outros campos
}

send_order(order_data, "YOUR_APP_ID", "YOUR_APP_TOKEN")
```

### Exemplo em cURL

```bash
curl -X POST https://api.deliveryzen.com.br/v1/orders \
  -H "Content-Type: application/json" \
  -H "APP_ID: YOUR_APP_ID" \
  -H "APP_TOKEN: YOUR_APP_TOKEN" \
  -d '{
    "id": "123123123",
    "ticketId": "001",
    "createdAt": "2025-07-12T21:51:39.204Z",
    "type": "delivery",
    "status": "PENDING",
    "subTotal": 21.00,
    "total": 21.00,
    "customer": {
      "id": "1",
      "name": "Bruno Frank",
      "phone": "62991724016"
    },
    "items": [
      {
        "id": 0,
        "name": "Dom Frank",
        "quantity": 1,
        "category": {
          "id": 1,
          "name": "Bebidas"
        }
      }
    ]
  }'
```

## Resposta da API

### Sucesso (200)

```json
{
  "success": true,
  "orderId": "123123123",
  "message": "Pedido criado com sucesso",
  "timestamp": "2025-07-12T21:51:39.204Z"
}
```

### Erro (400/500)

```json
{
  "success": false,
  "error": "Campo obrigatório não informado",
  "field": "customer",
  "timestamp": "2025-07-12T21:51:39.204Z"
}
```

## Códigos de Status HTTP

| Código | Descrição                                       |
| ------ | ----------------------------------------------- |
| `200`  | Pedido criado com sucesso                       |
| `400`  | Dados inválidos ou campos obrigatórios ausentes |
| `401`  | APP_ID ou APP_TOKEN inválido                    |
| `403`  | Acesso negado                                   |
| `500`  | Erro interno do servidor                        |

## Validações

### Campos Obrigatórios

- `id` - Código único do pedido
- `type` - Tipo do pedido
- `status` - Status do pedido
- `customer` - Dados do cliente
- `items` - Lista de itens do pedido

### Validações Específicas

- `id` deve ser único
- `total` deve ser igual à soma de `subTotal + deliveryFee + additionalFees - discounts`
- `coordinates` deve ser um array com latitude e longitude válidas
- `phone` deve estar no formato brasileiro (DDD + número)

## Rate Limiting

A API possui limite de 100 requisições por minuto por APP_ID.

## Tratamento de Erros

### Timeout

A API possui timeout de 30 segundos para processar cada requisição.

### Retry

Em caso de erro temporário (5xx), implemente retry com backoff exponencial:

- 1ª tentativa: imediata
- 2ª tentativa: após 1 segundo
- 3ª tentativa: após 2 segundos
- 4ª tentativa: após 4 segundos

## Segurança

### Autenticação

Sempre use HTTPS e inclua o APP_ID e APP_TOKEN nos headers.

### Validação

Valide todos os dados antes de enviar para a API.

### Logs

Mantenha logs detalhados das requisições enviadas para debug e auditoria.

## Testes

### Ambiente de Teste

Para testar a API, você pode usar:

- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)
- [cURL](https://curl.se/)

### Dados de Teste

Use IDs únicos para cada teste e dados fictícios para evitar conflitos.

## Suporte

Para dúvidas sobre a API, entre em contato com nossa equipe de suporte:

- Email: suporte@deliveryzen.com.br
- Documentação: [docs.deliveryzen.com.br](https://docs.deliveryzen.com.br)
