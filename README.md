# Delivery Zen API Documentation

> Documentação completa da API do Delivery Zen

## Sobre

Esta é a documentação oficial da API do Delivery Zen, construída com [docsify](https://docsify.js.org/).

## Como Visualizar

### Opção 1: Servidor Local

1. Instale o docsify globalmente:

```bash
npm i docsify-cli -g
```

2. Execute o servidor local:

```bash
docsify serve docs
```

3. Acesse: http://localhost:3000

### Opção 2: Servidor Python

```bash
cd docs
python -m http.server 8000
```

Acesse: http://localhost:8000

### Opção 3: Servidor Node.js

```bash
cd docs
npx serve .
```

## Estrutura

```
docs/
├── index.html          # Configuração do docsify
├── README.md           # Página inicial
├── _coverpage.md       # Página de capa
├── _sidebar.md         # Menu lateral
└── webhooks.md         # Documentação de webhooks
```

## Desenvolvimento

Para adicionar novas seções:

1. Crie um novo arquivo `.md` na pasta `docs/`
2. Adicione o link no `_sidebar.md`
3. Atualize o índice no `README.md` se necessário

## Tecnologias

- [docsify](https://docsify.js.org/) - Gerador de documentação
- [Vue.js Theme](https://docsify.js.org/#/themes) - Tema visual
- [Search Plugin](https://docsify.js.org/#/plugins?id=search) - Busca
- [Copy Code Plugin](https://github.com/jperasmus/docsify-copy-code) - Copiar código
- [Pagination Plugin](https://github.com/imyelo/docsify-pagination) - Navegação

## Contribuição

1. Faça um fork do projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## Licença

Este projeto está sob a licença MIT.
