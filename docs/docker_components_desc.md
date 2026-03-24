# Descrição dos Componentes do Docker

Este documento detalha os componentes definidos no arquivo `docker-compose.yaml` do projeto, explicando a função de cada serviço e as decisões de arquitetura.

## Visão Geral da Arquitetura

A arquitetura do sistema é baseada em microserviços, onde cada serviço é responsável por uma parte específica do negócio. Essa abordagem oferece flexibilidade, escalabilidade e resiliência. A comunicação entre os serviços é facilitada por um API Gateway (Kong) e um sistema de mensageria (RabbitMQ).

## Componentes

### Kong (API Gateway)

O `kong-service` atua como um API Gateway, sendo o ponto de entrada único para todas as requisições externas.

**Vantagens do Kong:**

*   **Centralização do Acesso:** Simplifica o acesso aos microserviços, expondo uma única URL para os clientes.
*   **Segurança:** Permite a implementação de políticas de segurança, como autenticação, autorização e rate limiting, de forma centralizada.
*   **Monitoramento e Logging:** Facilita o monitoramento do tráfego e a coleta de logs de todas as requisições.
*   **Flexibilidade:** Suporta plugins para adicionar funcionalidades extras, como transformação de requisições e respostas.
*   **Descoberta de Serviços:** Integra-se com ferramentas de descoberta de serviços para rotear o tráfego dinamicamente.

### RabbitMQ (Mensageria)

O `rabbitmq` é um message broker que permite a comunicação assíncrona entre os microserviços.

**Vantagens:**

*   **Desacoplamento:** Os serviços não precisam se comunicar diretamente, o que aumenta a resiliência do sistema.
*   **Escalabilidade:** Permite que os serviços sejam escalados de forma independente, sem impactar a comunicação.
*   **Confiabilidade:** Garante a entrega de mensagens, mesmo que um serviço esteja temporariamente indisponível.

### Bancos de Dados (PostgreSQL)

Cada microserviço (`iam`, `restaurant`, `order`, `payment`) possui sua própria instância de banco de dados PostgreSQL.

**Vantagens de um Banco de Dados por Serviço:**

*   **Isolamento:** Evita que um serviço afete o desempenho ou a disponibilidade de outro.
*   **Flexibilidade:** Permite que cada serviço escolha a tecnologia de banco de dados mais adequada para suas necessidades (embora aqui todos usem PostgreSQL).
*   **Escalabilidade:** Facilita o escalonamento de cada banco de dados de forma independente.
*   **Manutenção:** Simplifica a manutenção e a evolução do esquema de cada banco de dados.

### Microserviços

*   **identity-access-management:** Responsável pela autenticação e autorização de usuários.
*   **restaurant:** Gerencia informações sobre restaurantes, produtos e cardápios.
*   **order:** Processa e gerencia os pedidos dos clientes.
*   **payment:** Lida com o processamento de pagamentos dos pedidos.

### Build e Publicação dos Microserviços no Docker Hub

Os microserviços da aplicação são buildados como imagens Docker e publicados no Docker Hub.

**Vantagens:**

*   **Portabilidade:** As imagens Docker podem ser executadas em qualquer ambiente que tenha o Docker instalado, garantindo consistência entre os ambientes de desenvolvimento, teste e produção.
*   **Versionamento:** O Docker Hub permite o versionamento das imagens, facilitando o controle de qual versão de cada microserviço está em execução.
*   **Distribuição Simplificada:** Facilita a distribuição e o deploy dos microserviços, pois basta puxar a imagem do Docker Hub para o ambiente de destino.
*   **Integração Contínua e Deploy Contínuo (CI/CD):** A publicação no Docker Hub se integra facilmente com pipelines de CI/CD, automatizando o processo de build, teste e deploy dos microserviços.

### procpag (Simulador de API de Pagamento Externo)

O `procpag` é um container que simula uma API de pagamento externa. Ele foi fornecido para testar a integração do serviço de pagamento com um sistema externo.

**Comunicação e Rede:**

O `procpag` está em uma rede separada (`external-network`), e apenas o Kong consegue se comunicar com ele. Isso simula um cenário real onde a comunicação com um serviço externo é feita através de um gateway, garantindo que a comunicação interna da aplicação permaneça isolada e segura.
