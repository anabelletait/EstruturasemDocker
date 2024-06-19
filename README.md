# EstruturasemDocker

# Introdução

Este arquivo configura um ambiente completo para hospedar um site WordPress com suporte de banco de dados MySQL e caching via Redis. Inclui também ferramentas para monitoramento (Prometheus) e análise de desempenho dos containers (cAdvisor). Este guia descreve detalhadamente cada componente da configuração, suas funções e como eles interagem dentro do ambiente.

## Componentes do Sistema

### 1. Serviço WordPress

O serviço WordPress é o coração deste ambiente, responsável pela exibição do site e gerenciamento de conteúdo.

- **Imagem Docker:** `wordpress:latest`
  - A última versão do WordPress disponível para garantir atualizações de segurança e novas funcionalidades.
- **Configuração de Portas:**
  - `8081:80` mapeia a porta 8081 do host para a porta 80 do container, permitindo acesso externo via `http://localhost:8081`.
- **Variáveis de Ambiente:**
  - Configurações que definem como o WordPress se conecta ao banco de dados MySQL e ao Redis.
- **Volumes:**
  - `wordpress_data:/var/www/html` para persistência de dados e conteúdos do site.
- **Dependências:**
  - Garante que o WordPress só seja iniciado após o MySQL e o Redis estarem operacionais.

### 2. Serviço MySQL (db)

Fornece o armazenamento de dados para o WordPress.

- **Imagem Docker:** `mysql:latest`
- **Configurações de Segurança:**
  - `MYSQL_ROOT_PASSWORD` para a conta administrativa, e outras variáveis para a conta de usuário específica do WordPress.
- **Persistência de Dados:**
  - `db_data:/var/lib/mysql` para garantir que os dados do banco sejam mantidos entre reinicializações.

### 3. Serviço Redis

Utilizado para cache, melhorando a performance do WordPress.

- **Imagem Docker:** `redis:latest`
- **Configuração de Portas:**
  - A porta 6379 está exposta para comunicação entre o Redis e o WordPress.

### 4. Serviço Prometheus

Monitora e coleta métricas de todos os serviços rodando no Docker.

- **Imagem Docker:** `prom/prometheus:latest`
- **Acesso:**
  - `9090:9090` para visualizar o dashboard do Prometheus.
- **Configurações:**
  - Usa um arquivo de configuração específico localizado em `./prometheus/`.

### 5. Serviço cAdvisor

Fornece informações detalhadas sobre o uso de recursos por cada container.

- **Imagem Docker:** `gcr.io/cadvisor/cadvisor:latest`
- **Acesso:**
  - `8080:8080` para acessar a interface web do cAdvisor.
- **Monitoramento:**
  - Coleta dados dos containers para ajudar na otimização e no diagnóstico de problemas.

## Configuração de Volumes

Os volumes são essenciais para a persistência de dados, configurações e para garantir que informações essenciais não se percam em reinicializações:

- `wordpress_data`: para arquivos do WordPress.
- `db_data`: para dados do MySQL.
- `redis_data`: para dados do Redis.
- `prometheus_data`: para dados de configuração e métricas do Prometheus.

## Inicialização e Gestão dos Serviços

Para iniciar todos os serviços configurados no `docker-compose.yml`, use:

```bash
docker-compose up -d

Para interromper e remover todos os serviços, use:

docker-compose down

