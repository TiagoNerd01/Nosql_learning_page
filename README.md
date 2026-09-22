# Introdução a Nosql/Mongodb
## O que é Nosql?
- **NoSQL** é um **paradigma de banco de dados** que engloba diversos tipos de bancos de dados não relacionais.
- Projetados para oferecer:
   - Flexibilidade
   - Escalabilidade
   - Alto desepenho
##  Os quatro paradigmas de banco de dados NoSql são:
   - **Banco de dados orientados a documentos** (ex: mongoDb)
   - **Banco de dados chave-valor** (ex: Redis)
   - **Bancos de dados de famílias de colunas (wide-column)** (ex: Cassandra)
   - **Bancos de dados orientados a grafos** (ex.: Neo4j)
# O que é MongoDB?
   - O que significa **"Mongo"**?
      - o **Humongous** (Gigante) — o **MongoDB** foi projetado para armazenar e gerenciar grandes volumes de dados de forma eficiente.
   - MongoDB é um banco de dados NoSQL de **código aberto, orientado a documentos**, projetado para armazenar e gerenciar grandes quantidades de dados de maneira eficiente.
   - Diferentemente dos bancos de dados relacionais tradicionais (como MySQL ou PostgreSQL), o MongoDB **armazena os dados em documentos**, em vez de linhas em tabelas
# Como o MongoDB funciona?
   - Um servidor MongoDB pode hospedar múltiplos bancos de dados.
   - Cada banco de dados contém coleções (collections), e cada coleção armazena documentos (documents).
   - Todo registro no MongoDB é, na verdade, um **documento**.
   - Os documentos são armazenados no MongoDB em um formato semelhante ao JSON, chamado **BSON (Binary JSON)**.
   - Os documentos **BSON** são objetos que contêm uma lista ordenada dos elementos que armazenam.
   - Cada elemento é composto por um **nome de campo (field name)** e um **valor** de um determinado tipo.
## Relacionamentos
   - Diferentemente dos bancos de dados relacionais, o MongoDB **minimiza o uso de relacionamentos entre coleções**.
   - **Em vez de dividir** os dados relacionados em várias tabelas e depois uni-los por meio de JOINs, o MongoDB geralmente **armazena esses dados juntos** no mesmo documento utilizando documentos incorporados (embedded documents).
## Controles principais
   - mongosh
   - show databases / show dbs
   - use shop
   - show collections
   - db.<collection_name>.insertOne({<object>>})
   - db.createCollection("<collection_name>")
   - db.<collection_name>.find()
## Como funciona o arquivo Json
   - name": "jefté" é chamado de campo (field) ou propriedade (property) do documento JSON. Múltiplos campos são separados por vírgulas.
   - Os **campos (fields)** são compostos por uma **chave (key)**, também chamada de **nome (name)**, e um **valor (value)**. A chave e o valor são separados por dois-pontos (:).
   - Os **valores (values)** podem ser **strings** (por exemplo, "Jefté"), **números** (por exemplo, 35), **booleanos** (por exemplo, true), **arrays** ([... ]) e **outros documentos** (também chamados de **objetos**; { ... }).
![foto](./assets/crud-operations.png)

   - Exibir os bancos de dados
         - show databases

   - Criar banco de dados
         - use loja_informatica

   - Criar nova collection
         - db.createCollection("cliente")

   - Mostar todas as collections
         - show collections

   - Mostrar todos os documentos/objetos
         - db.cliente.find()

   - Insere apenas 1 document (objeto)
       - db.cliente.insertOne({   "nome": "jefté",   "idade": 35,   "pets": ["dora", "sabrina"],      "endereco": {    "logradouro": "Sossego"   }})

   - Inserir Muitos documents de uma vez
         - db.cliente.insertMany([{ "nome": "Brenno"}, { "nome": "João"}, { "nome": "MAria"}, { "nome": "José"}, { "nome": "Noé"}])

   - Buscar pelo campo
      - db.cliente.find({"nome": "José"})

   - Buscar pelo identificador único
      - db.cliente.find({_id: ObjectId('6a7bbab007ff2cf8649f68a9'),})

# Relacionamentos   
## Relacionamento One-to-One (1:1)
Ocorre quando um registro se relaciona exclusivamente com outro único registro [8].

**Embarcado (Embedded):** Indicado quando os dados pertencem exclusivamente àquela entidade e são lidos juntos [8].
javascript
Exemplo: Paciente ↔ Doença (Resumo médico)
db.patients.insertOne({
  name: "Jefté",
  age: 35,
  diseaseSummary: {
    diseases: ["cold", "broken leg"]
  }
})



**Por Referência:** Indicado quando as entidades possuem vida autônoma na aplicação[4].


 Exemplo: Pessoa ↔ Carro
db.persons.insertOne({
  name: "Jefté",
  age: 35,
  salary: 3000
})

db.cars.insertOne({
  model: "BMW",
  price: 40000,
  owner: ObjectId('6aa9e2cee9c288ce1241317e')
})




## 2\. Relacionamento One-to-Many (1:N)

Ocorre quando um único registro se relaciona com múltiplos registros secundários[5].

**Embarcado (Embedded):** Armazena as entidades dependentes dentro de um array de objetos[5][6].

Exemplo: Tópico ↔ Respostas
db.questionThreads.insertOne({
  creator: "Jefté",
  question: "How does that work?",
  answers: [
    { text: "Like that." },
    { text: "Thanks!" }
  ]
})


 **Por Referência:** Usado quando a subcoleção pode crescer indefinidamente, evitando estourar o **limite de 16MB por documento**[7][5].


 Exemplo: Cidade ↔ Cidadãos
db.cities.insertOne({
  name: "New York City",
  coordinates: { lat: 21, lng: 55 }
})

db.citizens.insertMany([
  { name: "Jefté Goes", cityId: ObjectId("5b98d6b44d01c52e1637a99f") },
  { name: "Brenno Salvador", cityId: ObjectId("5b98d6b44d01c52e1637a99f") }
])




## 3 Relacionamento Many-to-Many (N:M)

Ocorre quando múltiplos registros de uma coleção se associam a múltiplos registros de outra[8].

 **Embarcado (Embedded):** O histórico de itens é congelado dentro do próprio documento do cliente[8].
 Exemplo: Cliente ↔ Pedidos
db.customers.insertOne({
  name: "Jefté",
  age: 35
})

db.customers.updateOne(
  {},
  { $set: { orders: [{ title: "A Book", price: 12.99, quantity: 2 }] } }
)



**Por Referência:** Utiliza um array de ObjectId para relacionamentos cruzados[8][9].



