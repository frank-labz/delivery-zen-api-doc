# Delivery Zen API Documentation

> Documentação completa da API do Delivery Zen

## Índice

- [Introdução](README.md)
- [API de Pedidos](orders-api.md)
- [Webhooks](webhooks.md)

## Introdução

Bem-vindo à documentação da API do Delivery Zen. Esta documentação fornece informações detalhadas sobre como integrar com nossa plataforma.

### Sobre a API

A API do Delivery Zen permite que você integre seu sistema com nossa plataforma de delivery, enviando pedidos em tempo real e gerenciando o fluxo de trabalho.

### Autenticação

Todas as requisições para a API devem incluir o APP_ID e APP_TOKEN nos headers.

```http
APP_ID: YOUR_APP_ID
APP_TOKEN: YOUR_APP_TOKEN
```

### Base URL

```
https://api.deliveryzen.com.br/v1
```

## Próximos Passos

- [API de Pedidos](orders-api.md) - Aprenda como enviar pedidos para a plataforma
- [Webhooks](webhooks.md) - Saiba como receber notificações de atualização de pedidos
