# Webhooks

> Receba notificações automáticas do Delivery Zen sobre atualizações de pedidos

## Visão Geral

O Delivery Zen pode notificar automaticamente o seu sistema sempre que houver uma atualização de status de pedido. Para isso, basta informar a URL do seu sistema no campo **webhooks** da sessão de integrações do Delivery Zen.

Quando um pedido for atualizado, enviaremos uma requisição HTTP POST para a URL cadastrada, com o seguinte payload:

### Exemplo de Payload

```json
{
  "event": "order_updated",
  "payload": {
    "id": "123",
    "external_id": "963",
    "status": "preparing"
  }
}
```

- `id`: Código do pedido no Delivery Zen
- `external_id`: Código do pedido no sistema do integrador (se informado no momento da criação)
- `status`: Novo status do pedido

### Status possíveis

- `preparing` — Pedido está sendo preparado
- `done` — Pedido pronto
- `in_route` — Pedido saiu para entrega
- `delivered` — Pedido entregue ao cliente
- `canceled` — Pedido cancelado

## Como configurar

1. Acesse o painel do Delivery Zen
2. Vá até a sessão **Integrações**
3. Informe a URL do seu sistema no campo **webhooks**
4. Salve as alterações

## Exemplo de recebimento (Node.js)

```javascript
const express = require("express");
const app = express();
app.use(express.json());

app.post("/webhook", (req, res) => {
  const { event, payload } = req.body;
  if (event === "order_updated") {
    console.log("Pedido atualizado:", payload);
    // Atualize o pedido no seu sistema
  }
  res.status(200).send("ok");
});

app.listen(3000, () => console.log("Webhook listener rodando!"));
```

## Exemplo de recebimento (Python Flask)

```python
from flask import Flask, request
app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def webhook():
    data = request.json
    if data.get('event') == 'order_updated':
        payload = data.get('payload')
        print('Pedido atualizado:', payload)
        # Atualize o pedido no seu sistema
    return 'ok', 200

if __name__ == '__main__':
    app.run(port=3000)
```

## Resposta esperada

Sua aplicação deve responder com HTTP 200 para confirmar o recebimento do webhook.

## Segurança

- Recomendamos validar a origem das requisições (IP, headers, etc)
- Sempre utilize HTTPS para sua URL de webhook

## Suporte

Em caso de dúvidas, entre em contato com nosso suporte: suporte@deliveryzen.com.br
