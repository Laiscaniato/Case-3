Link do github para poder visualizar o trabalho na integra: https://github.com/Laiscaniato/Case-3

# MBA Engenharia de dados
## Trabalho de modelagem de dados 


| Nome dos integrantes do grupo |
|-------------------------------|
| Gabriela Gadelha Araújo       |
| Lais Helena Caniato           |


### Case 

* Crie sua missão, visão e valores
* Identifique quais são os KPIs do seu modelo de negócio
* Identifique quais dos seus dados devem ser utilizados para medir os KPIs
* Faça uma remodelagem do seu DW contendo apenas os Dados Mestres

**Missão:** Proporcionar saúde e bem estar com responsabilidade 


**Visão:**  Ser reconhecida pelo atendimento e estrutura oferecida, sendo admirado pelo cliente


**Valores:** Inovação, Transparência e respeito 

## KPIs do modelo de negócio

KPIs São métricas para medir o desempenho de uma empresa e ajudam a avaliar se a organização está seguindo o caminho certo.
Considerando que a empresa em questão é uma Academia, as métricas mais relevantes são:

**Taxa de Leads** : Essa métrica nos informa quantos clientes demosntraram interesse na academia, ou seja, os clientes em potencial

**Taxa de conversão** : Visa medir quantos leads se viraram negócio, ou seja, quantos desses leads se tornaram alunos
Isso possibilita acompanhar o time de vendas e se as estratégias estão funcionando ou não

**Custo de aquisição por cliente**: Quanto dinheiro esta sendo investido para trazer um novo aluno

**Taxa de retenção**: Investir esforços para que o aluno permaneça 

**Churn**: Avalia a taxa de cancelamentos, sendo importante para entender o que esta motivando o cancelamento e criar estratégias de retenção.

**Retorno sobre Investimento**: Aqui é possivel medir o retorno sobre o investimento que esta sendo feito


## Dados que devem ser utilizados para medir KPI
Para medir os KPIs citados, deve ser utilizado

* Nome 
* Sobrenome
* Status
* Contratado em
* Cancelado em
* Valor
* Preço
  
### Remodelagem do DW

### ALUNOS
  
| Nome Lógico       | Nome Físico       | 
|-------------------|-------------------|
| Identificador     | `id`              | 
| Nome              | `nome`            |  
| Sobrenome         | `sobrenome`       | 
| Data de Nascimento| `data_nascimento` | 
| Sexo              | `sexo`            | 


### Contratação 

| Nome Lógico       | Nome Físico       | 
|-------------------|-------------------|
| Identificador     | `id`              | 


### Preço
| Nome Lógico       | Nome Físico       | 
|-------------------|-------------------|
| Identificador     | `id`              | 
| Plano             | `plano`           | 



### Plano
| Nome Lógico       | Nome Físico       | 
|-------------------|-------------------|
| Identificador     | `id`              | 
| Nome              | `nome`            | 


## Glossário de Dados


| Nome Lógico       | Nome Físico       | Tipo de Dado    | Descrição                                      |
|-------------------|-------------------|-----------------|------------------------------------------------|
| Identificador     | `id`              | `INT`           | Identificador único do aluno. Chave primária   |
| Nome              | `nome`            | `VARCHAR(100)`  | Primeiro nome do aluno                         |
| Sobrenome         | `sobrenome`       | `VARCHAR(100)`  | Sobrenome do aluno                             |
| Data de Nascimento| `data_nascimento` | `DATE`          | Data de nascimento do aluno                    |
| Sexo              | `sexo`            | `CHAR(1)`       | Sexo do alu 'M' (masculino) ou 'F' (feminino)  |
| Preço             | `preco`           | `INTEGER`       | Valor da contratação do plano                  |
| Status            | `status`          | `VARCHAR(50)`   | Status atual da contratação do plano           |



### Detalhes dos Tipos de Dados

- **`INT`**: Tipo de dado numérico inteiro.
- **`VARCHAR(n)`**: Tipo de dado de texto variável, onde `n` define o comprimento máximo da string. 
- **`DATE`**: Tipo de dado para armazenar datas no formato YYYY-MM-DD.
- **`CHAR(1)`**: Tipo de dado de texto com comprimento fixo, onde `1` define quantos caracteres.
- **`FLOAT`**: Tipo de dado numérico que pode armazenar números com casas decimais.


## Tabelas Fato e Dimensão

![Logo do GitHub](https://github.com/Laiscaniato/Case-3/blob/main/Dimensional%20dados%20mestres.png)

## Código SQL da tabela Fato

```sql
-- Criação de tipos ENUM
CREATE TYPE "possibilidades_status" AS ENUM (
  'ativo',
  'pausado',
  'cancelado'
);

CREATE TYPE "possibilidades_plano" AS ENUM (
  'mensal',
  'trimestral',
  'semestral',
  'anual'
);

-- Criação das tabelas
CREATE TABLE "fato" (
  "Aluno_id" serial,
  "Contratação_id" serial,
  "Preço_id" serial,
  "Plano_id" serial
);

CREATE TABLE "dim_aluno" (
  "id" serial PRIMARY KEY,
  "nome" varchar(100) NOT NULL,
  "sobrenome" varchar(100) NOT NULL,
  "data_nascimento" date NOT NULL,
  "sexo" char NOT NULL
);

CREATE TABLE "dim_contratação" (
  "id" serial PRIMARY KEY
);

CREATE TABLE "dim_preço" (
  "id" serial PRIMARY KEY,
  "plano" varchar(100) NOT NULL
);

CREATE TABLE "dim_plano" (
  "id" serial PRIMARY KEY,
  "nome" possibilidades_plano(128)
);

-- Adicionando chaves estrangeiras
ALTER TABLE "fato" ADD FOREIGN KEY ("Aluno_id") REFERENCES "dim_aluno" ("id");

ALTER TABLE "fato" ADD FOREIGN KEY ("Contratação_id") REFERENCES "dim_contratação" ("id");

ALTER TABLE "fato" ADD FOREIGN KEY ("Preço_id") REFERENCES "dim_preço" ("id");

ALTER TABLE "fato" ADD FOREIGN KEY ("Plano_id") REFERENCES "dim_plano" ("id");
