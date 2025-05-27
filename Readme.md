# PROVA DE DEVOPS - Universidade Positivo 
**01.2025**

## Dockerfiles

Cada API possui seu próprio Dockerfile configurado para seu ambiente específico:

* **API de Produtos (Node.js):** Utiliza a imagem oficial do Node.js, pra instalar dependências e executar a aplicação na porta 3001. O Dockerfile copia o código e prepara o ambiente para rodar o servidor Express que responde as requests de products.

* **API de Pedidos (Python):** Instala as libs necessárias (Flask, Redis, MySQL Connector) e executa a aplicação Flask na porta 3002. Essa API ficou responsável por criar pedidos, consultar a API de Produtos e salvar os dados no banco.

* **API de Pagamentos (PHP puro):** Usa a imagem do PHP CLI para rodar um servidor PHP na porta 3003. Essa API acessa a API de Pedidos para obter informações do pedido e retorna o status do pagamento.

## Docker Compose

O `docker-compose.yml` faz a orquestração dos 3 serviços.

* **products:** Serviço que roda a API de Produtos. A porta 3001 do container é mapeada para o host para permitir acesso externo.

* **orders:** Serviço que roda a API de Pedidos, que depende dos serviços de produtos, banco de dados (`db`) e cache (`redis`). Possui variáveis de ambiente para configurar conexão com o Redis e com o banco. 

* **payments:** Serviço da API de Pagamentos, que depende do serviço orders. 

* **db:** Serviço do banco MySQL, configurado com usuário root, senha `example` e banco `ecommerce`. faz a persistência dos dados e a porta 3307 no host pra evitar conflito com outros serviços.

* **redis:** Serviço Redis, usado para cachear as respostas da API de Produtos, melhorando a performance das chamadas subsequentes.

A rede Docker (`app-network`) conecta todos os serviços juntos permitindo comunicação interna por nomes do serviço.