# Pedidos

> Gerencie pedidos na plataforma Delivery Zen

## Visão Geral

A seção de Pedidos da API do Delivery Zen permite que você gerencie completamente o ciclo de vida dos pedidos, desde a criação até a entrega. Aqui você encontrará todas as funcionalidades relacionadas aos pedidos.

## Funcionalidades Disponíveis

### [Criar Pedidos](create.md)

Envie novos pedidos para a plataforma Delivery Zen. Esta API permite criar pedidos com todos os detalhes necessários como cliente, endereço, itens, pagamento e observações.

**Endpoint**: `POST https://api.deliveryzen.com.br/v1/orders`

### [Alterar Status](update-status.md)

Atualize o status de pedidos existentes na plataforma. Útil para sincronizar o progresso do pedido entre seu sistema e o Delivery Zen.

**Endpoint**: `PUT https://api.deliveryzen.com.br/v1/orders/:id/change_status`

## Fluxo Típico de um Pedido

1. **Criação**: Use a API de Criar Pedidos para enviar um novo pedido
2. **Acompanhamento**: Monitore as atualizações através de [Webhooks](../webhooks.md)
3. **Atualização**: Use a API de Alterar Status para atualizar o progresso
4. **Finalização**: Marque o pedido como entregue quando concluído

## Autenticação

Todas as APIs de pedidos requerem autenticação via APP_ID e APP_TOKEN nos headers:

```http
APP_ID: YOUR_APP_ID
APP_TOKEN: YOUR_APP_TOKEN
```

## Status de Pedidos

| Status      | Descrição                        |
| ----------- | -------------------------------- |
| `PENDING`   | Pedido pendente (status inicial) |
| `preparing` | Pedido está sendo preparado      |
| `done`      | Pedido pronto                    |
| `in_route`  | Pedido saiu para entrega         |
| `delivered` | Pedido entregue ao cliente       |
| `canceled`  | Pedido cancelado                 |

## Próximos Passos

- [Criar Pedidos](create.md) - Aprenda como enviar novos pedidos
- [Alterar Status](update-status.md) - Atualize o status dos pedidos
- [Webhooks](../webhooks.md) - Receba notificações automáticas
