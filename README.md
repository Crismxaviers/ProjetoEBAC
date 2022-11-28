# Projeto-EBAC
Projeto final do curso de SQL para Analise de Dados da EBAC

# O PROJETO

Observação: Não há necessidade de entrega de exercício nessa fase.  O que vocês precisam fazer é o projeto!

**Crie um bloco de notas e insira o link do seu projeto para o tutor avaliar!**

Não esqueça de deixar o portfólio público! Você pode postar o link com uma pequena descrição nas suas redes sociais (Facebook, Linkedlin, Twitter, ...), prática altamente recomendável

# **1. Criação da tabela e algumas queries**



> **Não há necessidade de entrega**. 
Para esses exercícios, utilizaremos os mesmo dados do módulo 3.

As informações de **credito8.csv** estarão disponíveis no material de aula.

Siga os seguintes passos para criação da tabela (desconsidere caso você já tenha os dados no seu S3):

* Crie uma pasta bucket-transacoes no seu S3 e carregue o arquivo **credito8.csv**
* Volta para o AWS Athena e execute o seguinte comando:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.credito ( 
  `idade` int,
  `sexo` string,
  `dependentes` int,
  `escolaridade` string,
  `estado_civil` string,
  `salario_anual` string,
  `tipo_cartao` string, 
  `qtd_produtos` bigint,
  `iteracoes_12m` int,
  `meses_inativo_12m` int,
  `limite_credito` float,
  `valor_transacoes_12m` float,
  `qtd_transacoes_12m` int 
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = ',',
  'field.delim' = ','
) LOCATION <sua-localizacao>
TBLPROPERTIES ('has_encrypted_data'='false');
```

Utilizaremos a tabela para o projeto.

Abaixo vão algumas dicas de queries que podem ser utilizadas. Sugiro que crie novas, com outras funcionalidades não utilizadas aqui e faça novas perguntas para o dataset.

```sql
select * from credito limit 10;
```

```sql
select count(*) from credito
```
```sql
DESCRIBE credito
```

```sql
SELECT DISTINCT escolaridade FROM credito
```

```sql
select count(*), salario_anual from credito group by salario_anual
```

```sql
select count(*), sexo from credito group by sexo
```

```sql
select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo from credito 
where escolaridade != 'na' and tipo_cartao != 'na' 
group by escolaridade, tipo_cartao, sexo 
order by limite_credito desc 
limit 10
```

```sql
select max(valor_transacoes_12m) as maior_valor_gasto, avg(valor_transacoes_12m) as media_valor_gasto, min(valor_transacoes_12m) as min_valor_gasto, sexo 
from credito 
group by sexo
```

```sql
select avg(qtd_produtos) as qts_produtos, avg(valor_transacoes_12m) as media_valor_transacoes, avg(limite_credito) as media_limite, sexo, salario_anual 
from credito 
where salario_anual != 'na' 
group by sexo, salario_anual 
order by avg(valor_transacoes_12m) desc
```
