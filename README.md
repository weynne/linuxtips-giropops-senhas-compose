# LINUXtips-Giropops-Senhas-Compose

### **DESAFIO PICK 2024.2**

## Resolução com Docker Compose

###### Este guia se concentra na utilização do Docker Compose para subir a aplicação. As etapas de instalação, bem como a otimização da imagem com Distroless e o envio para o Docker Hub, podem ser consultadas nos seus repositórios.`.

1. Criar o arquivo `docker-compose.yml`
- Crie um arquivo chamado docker-compose.yml na raiz do seu projeto com o seguinte conteúdo:
```
services:
  giropops-senhas:
    build:
      context: ./Dockerfiles  # Caminho para o seu Dockerfile
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    networks:
      - giropops
    environment:
      REDIS_HOST: redis
    volumes:
      - strigus:/strigus  # Configure o volume conforme necessário
    depends_on:
      - redis
    deploy:
      resources:
        reservations:
          cpus: '0.25'
          memory: 128M
        limits:
          cpus: '0.5'
          memory: 256M

  redis:
    image: cgr.dev/chainguard/redis:7.0.11-stable  # Use uma tag específica
    networks:
      - giropops
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s

networks:
  giropops:
    driver: bridge

volumes:
  strigus:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /caminho/para/o/diretorio 
```
2. Subir a aplicação:
- Execute o seguinte comando no terminal, na pasta onde se encontra o arquivo `docker-compose.yml`:
```
docker compose up -d
```
3. Acessar a aplicação::

- Acesse a aplicação no seu navegador através da URL: `localhost:5000`.

4. Observações:

- Ajuste o caminho para o seu Dockerfile e a tag da imagem do Redis conforme necessário.
- Configure o volume strigus de acordo com suas necessidades, especificando o tipo de driver, as opções e o caminho para o diretório, se aplicável.
- Utilize o comando docker compose down para parar e remover os contêineres.

