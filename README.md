# Microservices Python

Este repositório demonstra uma arquitetura de microserviços em Python para processamento de vídeos, conversão para MP3 e notificação de usuários. O projeto utiliza Flask, RabbitMQ, MongoDB, MySQL e MoviePy, além de Docker e Kubernetes para orquestração.

## Propósito

O objetivo é mostrar como construir um pipeline de processamento de mídia desacoplado, onde:
- O usuário faz upload de um vídeo.
- O vídeo é armazenado e enviado para conversão.
- Um microserviço converte o vídeo em MP3.
- Outro microserviço notifica o usuário quando a conversão termina.

## Microserviços

- **auth**: Autenticação de usuários (Flask, MySQL).
- **gateway**: API Gateway para upload, autenticação e download (Flask, MongoDB, RabbitMQ).
- **converter**: Consome vídeos, converte para MP3 (MoviePy, MongoDB, RabbitMQ).
- **notification**: Envia notificações após conversão (RabbitMQ).
- **rabbit**: Manifests para deploy do RabbitMQ no Kubernetes.

## Tecnologias Utilizadas
- Python 3.10
- Flask
- Flask-MySQLdb, Flask-PyMongo
- MoviePy
- RabbitMQ
- MongoDB
- MySQL
- Docker, Docker Compose
- Kubernetes (manifests em cada serviço)

## Como Executar Localmente

### Pré-requisitos
- Docker e Docker Compose instalados
- (Opcional) Minikube ou outro cluster Kubernetes para testes com manifests

### Passos Básicos
1. Clone o repositório:
   ```bash
   git clone <url-do-repo>
   cd microservices_python/python
   ```
2. Configure variáveis de ambiente conforme os arquivos `manifests/configmap.yaml` e `secret.yaml` de cada serviço.
3. Construa as imagens Docker:
   ```bash
   docker build -t auth-service src/auth/
   docker build -t gateway-service src/gateway/
   docker build -t converter-service src/converter/
   docker build -t notification-service src/notification/
   ```
4. Suba os containers necessários (RabbitMQ, MongoDB, MySQL, serviços):
   ```bash
   # Exemplo usando docker-compose.yaml (crie conforme sua necessidade)
   docker-compose up
   ```
   Ou suba manualmente cada container conforme as portas e dependências.
5. Acesse o gateway (ex: http://localhost:8080) para autenticação, upload e download.

### Kubernetes
- Use os manifests em `src/<serviço>/manifests/` para deploy em cluster Kubernetes.
- Exemplo:
  ```bash
  kubectl apply -f src/auth/manifests/
  kubectl apply -f src/gateway/manifests/
  # ...repita para os demais
  ```

## Exemplo de Uso
1. Faça login via `/login` no gateway para obter um token JWT.
2. Faça upload de um vídeo via `/upload` (enviando o token JWT).
3. O vídeo será processado, convertido para MP3 e você será notificado ao final.

## Observações
- Ajuste as variáveis de ambiente conforme seu ambiente local.
- O banco MySQL pode ser inicializado com o script `src/auth/init.sql`.
- Os serviços se comunicam via filas RabbitMQ.

---

Sinta-se à vontade para abrir issues ou contribuir!
