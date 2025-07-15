# Alterar Status

> Atualize o status de um pedido existente no Delivery Zen

## Visão Geral

A API de Update Order Status permite que você altere o status de um pedido específico no Delivery Zen. Esta funcionalidade é útil para sincronizar o status do pedido entre seu sistema e a plataforma Delivery Zen.

## Endpoint

### URL da API

```
PUT https://api.deliveryzen.com.br/v1/orders/:id/change_status
```

### Método

```
PUT
```

### Headers

```http
Content-Type: application/json
APP_ID: YOUR_APP_ID
APP_TOKEN: YOUR_APP_TOKEN
```

### Parâmetros da URL

| Parâmetro | Tipo   | Descrição                        |
| --------- | ------ | -------------------------------- |
| `id`      | string | ID do pedido que será atualizado |

## Estrutura do Payload

### Exemplo de Payload

```json
{
  "status": "preparing"
}
```

### Campos do Payload

| Campo    | Tipo   | Descrição             | Obrigatório |
| -------- | ------ | --------------------- | ----------- |
| `status` | string | Novo status do pedido | Sim         |

### Status Disponíveis

| Status     | Descrição                            |
| ---------- | ------------------------------------ |
| `created`  | Pedido criado aguardando confirmação |
| `waiting`  | Pedido e pronto para ser produzido   |
| `done`     | Pedido pronto para ser despachado    |
| `canceled` | Pedido cancelado                     |

## Implementação

### Exemplo em Node.js

```javascript
const axios = require("axios");

const updateOrderStatus = async (orderId, newStatus) => {
  try {
    const response = await axios.put(
      `https://api.deliveryzen.com.br/v1/orders/${orderId}/change_status`,
      {
        status: newStatus,
      },
      {
        headers: {
          "Content-Type": "application/json",
          APP_ID: "YOUR_APP_ID",
          APP_TOKEN: "YOUR_APP_TOKEN",
        },
      }
    );

    console.log("Status atualizado com sucesso:", response.data);
    return response.data;
  } catch (error) {
    console.error(
      "Erro ao atualizar status:",
      error.response?.data || error.message
    );
    throw error;
  }
};

// Exemplo de uso
updateOrderStatus("123123123", "preparing");
```

### Exemplo em Python

```python
import requests

def update_order_status(order_id, new_status, app_id, app_token):
    url = f"https://api.deliveryzen.com.br/v1/orders/{order_id}/change_status"

    payload = {
        "status": new_status
    }

    headers = {
        "Content-Type": "application/json",
        "APP_ID": app_id,
        "APP_TOKEN": app_token
    }

    try:
        response = requests.put(url, json=payload, headers=headers)
        response.raise_for_status()

        print("Status atualizado com sucesso:", response.json())
        return response.json()
    except requests.exceptions.RequestException as e:
        print("Erro ao atualizar status:", e)
        raise e

# Exemplo de uso
update_order_status('123123123', 'preparing', 'YOUR_APP_ID', 'YOUR_APP_TOKEN')
```

### Exemplo em cURL

```bash
curl -X PUT https://api.deliveryzen.com.br/v1/orders/123123123/change_status \
  -H "Content-Type: application/json" \
  -H "APP_ID: YOUR_APP_ID" \
  -H "APP_TOKEN: YOUR_APP_TOKEN" \
  -d '{
    "status": "preparing"
  }'
```

## Resposta da API

### Sucesso (200)

```json
{
  "success": true,
  "orderId": "123123123",
  "oldStatus": "PENDING",
  "newStatus": "preparing",
  "message": "Status atualizado com sucesso",
  "timestamp": "2025-07-12T21:51:39.204Z"
}
```

### Erro (400/404/500)

```json
{
  "success": false,
  "error": "Status inválido",
  "field": "status",
  "timestamp": "2025-07-12T21:51:39.204Z"
}
```

## Códigos de Status HTTP

| Código | Descrição                           |
| ------ | ----------------------------------- |
| `200`  | Status atualizado com sucesso       |
| `400`  | Status inválido ou dados incorretos |
| `401`  | APP_ID ou APP_TOKEN inválido        |
| `403`  | Acesso negado                       |
| `404`  | Pedido não encontrado               |
| `500`  | Erro interno do servidor            |

## Validações

### Campos Obrigatórios

- `status` - Deve ser um dos valores válidos listados acima

### Validações Específicas

- O pedido deve existir no sistema
- O status deve ser um dos valores permitidos
- O APP_ID e APP_TOKEN devem ser válidos

## Casos de Uso

### Fluxo Típico de Atualização

1. **Pedido Criado**: Status inicial `PENDING`
2. **Pedido Aceito**: Atualizar para `preparing`
3. **Pedido Pronto**: Atualizar para `done`
4. **Pedido em Entrega**: Atualizar para `in_route`
5. **Pedido Entregue**: Atualizar para `delivered`

### Exemplo de Fluxo Completo

```javascript
// Pedido criado
const orderId = "123123123";

// Aceitar pedido
await updateOrderStatus(orderId, "preparing");

// Pedido pronto
await updateOrderStatus(orderId, "done");

// Pedido em entrega
await updateOrderStatus(orderId, "in_route");

// Pedido entregue
await updateOrderStatus(orderId, "delivered");
```

## Tratamento de Erros

### Status Inválido

Se um status inválido for enviado, a API retornará erro 400:

```json
{
  "success": false,
  "error": "Status inválido. Valores permitidos: preparing, done, in_route, delivered, canceled",
  "field": "status"
}
```

### Pedido Não Encontrado

Se o ID do pedido não existir, a API retornará erro 404:

```json
{
  "success": false,
  "error": "Pedido não encontrado",
  "orderId": "999999999"
}
```

## Rate Limiting

A API possui limite de 100 requisições por minuto por APP_ID.

## Segurança

### Autenticação

Sempre use HTTPS e inclua o APP_ID e APP_TOKEN nos headers.

### Validação

Valide o status antes de enviar para a API.

### Logs

Mantenha logs detalhados das atualizações de status para auditoria.

## Testes

### Ambiente de Teste

Para testar a API, você pode usar:

- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)
- [cURL](https://curl.se/)

### Dados de Teste

Use IDs de pedidos válidos e status permitidos para os testes.

## Suporte

Para dúvidas sobre a API, entre em contato com nossa equipe de suporte:

- Email: suporte@deliveryzen.com.br
- Documentação: [docs.deliveryzen.com.br](https://docs.deliveryzen.com.br)
