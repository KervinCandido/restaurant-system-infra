# 🍴 Restaurant System Infrastructure

Este repositório centraliza a orquestração, configuração de rede e documentação do ecossistema de microserviços do Sistema de Gerenciamento de Restaurantes.

## 🏗 Arquitetura do Sistema

O sistema foi desenhado seguindo os princípios de microserviços, utilizando o **Kong** como API Gateway para centralizar a autenticação e o roteamento.

### Componentes:

| Serviço | Responsabilidade | Imagem Docker Hub |
| :--- | :--- | :--- |
| **Kong Gateway**       | Ponto de entrada único, Auth e Rate Limiting | `kong:3.9.1-ubuntu` |
| **Identity Service**   | Gestão de usuários, roles e tokens JWT       | `kervincandido/identity-access-management` |
| **Restaurant Service** | Gestão de cardápios, mesas e horários        | `kervincandido/restaurant` |
| **Order Service**      | Fluxo de pedidos e integração com cozinha    | `kervincandido/order` |
| **Payment Service**    | Integração com gateways de pagamento         | `seu-user/restaurant-payment-service` |

-----

## 🚀 Como Executar (Docker Compose)

### Pré-requisitos:

  * Docker e Docker Compose instalados.
  * Arquivo `.env` configurado na raiz (veja `.env.example`).

### Passo a passo:

1.  **Clone este repositório:**
    ```bash
    git clone https://github.com/KervinCandido/restaurant-system-infra.git && cd restaurant-system-infra
    ```
2.  **Suba o ecossistema:**
    ```bash
    docker-compose up -d
    ```
3.  **Verifique a saúde dos serviços:**
    ```bash
    docker-compose ps
    ```

-----

## 🛣 API Gateway & Rotas

O **Kong** está configurado no modo **DB-less**. Todas as rotas de consumo externo passam pela porta `80`.

  * **Identity API:** `http://localhost:8000/auth/*` -\> Direciona para `identity-iam:8080`
  * **Restaurant API:** `http://localhost:8000/restaurants/*` -\> Direciona para `restaurant:8080`
  * **Order API:** `http://localhost:8000/restaurants/{restaurant-id}/orders/*` -\> Direciona para `order:8080`

> **Nota:** Os serviços internos não devem ser expostos diretamente para fora da rede do Docker.

-----

## ⚙️ Configurações (.env)

O arquivo `.env` centraliza as credenciais e versões. **Nunca comite o `.env` real**, apenas o exemplo.

Exemplos de variáveis de ambiente no `.env.example`.

-----
