# Cqrs-quarkus-kafka

### Ferramentas necessárias:

* Java 14 ou versão superior
* Docker
* Kafka
* Amazon EC2
* Amazon RDS
* Postgresql
* Quarkus


## A aplicação

Simula um cenário de conta bancária em que um usuário final adiciona uma transação de receita ou despesa e é processado em uma fonte de eventos ascíncrona e arquitetura CQRS para recalcular o saldo da conta bancária do usuário. O usuário também pode solicitar o saldo de sua conta. Aqui embaixo você pode ver o design:

! [Design] (/ images / design.png)

## Implantando os serviços externos

`` `
docker-compose up -d --build
`` `

Ele implantará quatro contêineres docker em seu ambiente com MongoDB, PostgreSQL, Kafka e Zookepper (exigido por Kafka)

Depois de implantar o Kafka, você precisará [criar o tópico no cluster Kafka] (https://kafka.apache.org/quickstart).

## Testando o aplicativo

#### Executando uma solicitação CURL para criar uma transação de receita

`` `
curl -X POST -H "Content-Type: application / json" -d @ income-transaction.json http: // localhost: 8080 / transactions
`` `

#### Executando uma solicitação CURL para criar uma transação de despesas

`` `
curl -X POST -H "Content-Type: application / json" -d @ expens-transaction.json http: // localhost: 8080 / transactions
`` `

#### Executando solicitação CURL para buscar o equilíbrio

`` `
curl http: // localhost: 8081 / balance \? accountId \ = wesley | json_pp
`` `

#### Executando [K6's] (https://k6.io) teste de desempenho simples

`` ``
k6 run --vus 10 --duration 60s performance-tests / income.js
k6 run --vus 10 --duration 60s performance-tests / expens.js
`` ``
