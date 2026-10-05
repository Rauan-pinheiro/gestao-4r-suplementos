# 💪 4R Suplementos — Sistema de Gestão

![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=flat&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-6-092E20?style=flat&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=flat&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)
![Status](https://img.shields.io/badge/status-em%20produção-brightgreen)

Sistema web de gestão feito para uma **loja real de suplementos** que vende em vários pontos de venda. Ele substituiu o controle que era feito em arquivos de backup soltos e centraliza estoque, vendas, fiado, orçamentos e promoções em um só lugar.

<!-- Adicione aqui um print do dashboard: ![Dashboard](docs/dashboard.png) -->

## ✨ Funcionalidades

- **Dashboard** com faturamento, lucro e gráfico de vendas mês a mês
- **Estoque por local de venda**, com alerta de estoque mínimo e comparação de preços dentro de uma categoria
- **Controle de validade**, listando os produtos perto de vencer
- **Lançamento de vendas** com carrinho, várias formas de pagamento e baixa automática no estoque
- **Fiado e inadimplentes**: vendas não pagas ficam em aberto até serem marcadas como pagas
- **Orçamentos** que podem ser convertidos em venda com um clique
- **Promoções** por desconto percentual ou preço fechado, com período de validade
- **Histórico de vendas** e ficha de cada cliente
- **Importação de dados legados**: um comando próprio (`importar_backups`) migrou os backups JSON antigos da loja, corrigindo nomes de clientes grafados de formas diferentes

## 🧠 Decisões técnicas

- **Regra de negócio fora das views**: o registro de venda fica em `suplementos/services/vendas.py`, dentro de uma transação atômica (`@transaction.atomic`). Se algum item falhar, nem a venda nem a baixa de estoque são gravadas.
- **Snapshot de preço e nome**: cada item de venda guarda o nome e os preços de custo e venda do momento. Alterar um produto depois não muda o histórico nem o cálculo de lucro.
- **Views organizadas por domínio** (`views/produtos.py`, `views/vendas.py`, `views/orcamento.py`…) em vez de um único `views.py` grande.
- **Acesso restrito**: um middleware exige login em todas as páginas, já que é um sistema interno.
- **Configuração por variáveis de ambiente** com `django-environ`; nenhum segredo fica no código.

## 🛠️ Stack

| Camada | Tecnologias |
| :--- | :--- |
| Back-end | Python, Django 6, Gunicorn |
| Banco de dados | PostgreSQL 16 |
| Front-end | Django Templates, HTML, CSS, JavaScript |
| Infraestrutura | Docker, Docker Compose, Caddy (HTTPS automático), WhiteNoise |
| Deploy | VPS com Docker Compose ou Railway |

## 🚀 Como rodar localmente

Pré-requisito: Docker e Docker Compose.

```bash
git clone https://github.com/Rauan-pinheiro/gestao-4r-suplementos.git
cd gestao-4r-suplementos
cp .env.example .env      # preencha os valores
docker compose up --build
```

O superusuário é criado automaticamente a partir das variáveis `DJANGO_SUPERUSER_*` do `.env`. Depois de subir, acesse `http://localhost` e entre com essas credenciais.

O guia completo, com execução sem Docker, deploy e checklist de produção, está em [README_DEV.md](README_DEV.md).

## 📁 Estrutura

```
config/                 # settings, urls, wsgi
suplementos/
├── models.py           # Produto, Venda, Cliente, Orçamento, Promoção...
├── services/vendas.py  # regra de negócio do registro de venda
├── views/              # uma view por domínio
├── middleware.py       # exige login em todo o sistema
├── management/commands/importar_backups.py
└── templates/ static/
Dockerfile · docker-compose.yml · Caddyfile · entrypoint.sh
```

## 🤝 Desenvolvido em parceria com o Claude

Construí este sistema em parceria com o **Claude**, a IA da Anthropic, que trabalhou como meu par de programação. Eu conduzi o projeto: levantei as necessidades do negócio, tomei as decisões e validei tudo no uso real. O Claude me ajudou a desenhar a arquitetura, escrever e revisar código e documentar.


## 👨‍💻 Autor

**Rauan Pinheiro Lima**
[LinkedIn](https://linkedin.com/in/rauanpinheiro-dev) · [GitHub](https://github.com/Rauan-pinheiro)
