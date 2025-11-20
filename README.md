# Curso: Dominando o Databricks com Spark e Pyspark 
### - Instrutor: Fernando Amaral <br> - Plataforma: - Udemy <br> - [Link do curso](https://www.udemy.com/course/databricks-machinelearning/)

<br>

# Seções do curso
### __1. Introdução__ 
#### __1.1 Databricks__
- Pataforma __(PaaS)__ em nuvem que permite trabalhar com dados e inteligência artificial.
- Integra armazenamentom processamento e análise em um único ambiente.
- É possível escolher qual nuvem utilizar (AWS ou Azzure, por exemplo).

#### __1.2 Spark__
- Plataforma de computação em cluster tolerante a falhas.
- Open source.
- Processamento em memória.
- Suporta várias linguagens (Scala, Pyrhon, R, SQL).

### __2. Datawarehouse, Datalake, Delta Lake__
#### __2.1 Datawarehouse__
- É um repositório central de dados, que trabalha com dados estruturados, voltados para consulta e análise, visando a tomada de decisão.
- Dados tratados, consolidados e históricos em modelo fato e dimensão.
- Integra dados de várias fontes (ERPs, CRM).

#### __2.2 Datalake__
- Repositório de dados brutos para grandes volumes.
- Dados estruturados, semi-estruturados (JSON) e não estruturados (imagens, vídeos, logs)

#### __2.3 Data Lakehouse__
- União das ideias de Data Warehouse e Data Lake.
- Utiliza armazenamento de Data Lake (S#, por exemplo).
- ACID: <br> ✓ Atomicidade (transação deve acontecer de forma completa - tudo ou nada) <br> ✓ Consistência (mantém as regras válidas ) <br> ✓ Isolamento (transações em paralelo não se atrapalham) <br> ✓ Durabilidade (oersist~encia de dados)

#### __2.4 Delta Lake__
- Camada de armazenamento transacional em cima de um Data Lake.
- Solução open source, criada pela Databricks.
- Transações Acid
- Esquema de controle
- Histórico de versões
- Escalabilidade e desempenho
- Uperstes e exclusões
- Compatibilidade

### Formatos de armazenamento: 
- Parquet (armazenamento colunar e padrão do Spark)
- ORC (armazenamento colunar e padrão do Hive)
- Avro (armazenamento em linha)
- Muitos atributos e mais escrita – linha
- Menos atributos e mais leitura - coluna
- Armazenamento em linha: <br> ✓ Cada linha representa uma tupla. <br> ✓ Armazenados juntos em disco.  <br> ✓ Recupera-se a linha toda facilmente. <br> ✓ Ótimo para sistemas transacionais,
onde ocorrem atualizações e inclusões de linhas. <br> ✓ Compressão: como uma linha pode ter diferentes tipos, é menos eficiente.
- Armazenamento por Coluna  <br> ✓ Coluna é armazenada inteira em disco.  <br> ✓ Em uma consulta, apenas as colunas
necessárias são lidas.   <br> ✓ Otimizado para consulta e análise,
recuperando colunas inteiras com alta performance.  <br> ✓ Compressão: como a coluna tem um mesmo tipo, é mais eficiente.

### Sistemas de arquivos distribuídos: 
- Sistema de arquivos que permite que o acesso a arquivos e dados seja transparente, independentemente de onde os dados estejam fisicamente armazenados.
- Projetado para armazenar e analisar grandes volumes de dados de maneira distribuída e paralela em um cluster de computadores. Ele divide os arquivos em blocos, distribui-os em todo o cluster e mantém cópias redundantes para garantir a confiabilidade e a
disponibilidade dos dados.
- Exemplos: <br> ✓ S3 <br> ✓ Google File System <br> ✓ GlusterFS <br> ✓  HDFS

### DBFS (Databricks File System): 
- Abstração de armazenamento de objetos na nuvem, como o Amazon S3 ou Azure Blob Storage.
- Para tarefas de processamento de dados, o Databricks utiliza o Apache Spark.
- Fornece uma camada de armazenamento transacional ACID para aprimorar a confiabilidade e o desempenho do sistema de arquivos (Delta Lake).


### __3. Introdução o Spark__
#### __3.1 Spark__
- Plataforma de computação em cluster tolerante a falhas.
- Open source.
- Processamento em memória.
- Escalável, particionado, paralelismo
- Suporta várias linguagens (Scala, Pyrhon, R, SQL).

#### __3.2 Dataframe__
- Tabelas com linas e colunas.
- Dataframe são imutáveis.  Uma transformação gera um novo dataframe.
- Com schema conhecdo.
- Colunas podem ter tipos diferentes.
- Existência de análises comuns: agrupar, ordenar, filtrar.
- O Spark pode otimizar análises através de planos de execução.

#### __3.2 Lazy Evaluation__
No Spark, **lazy evaluation** (avaliação preguiçosa) significa:

> O Spark **não executa** suas operações assim que você escreve o código.
> Ele só **executa de verdade** quando você manda uma *ação* (action).

#### __3.2.1. Transformations x Actions__

No Spark temos:

#### ✅ Transformations (preguiçosas)

Ex.: `select`, `filter`, `withColumn`, `groupBy`, `join`, etc.

* Quando se faz:

  ```python
  df2 = df.filter(df.id > 10)
  df3 = df2.select("id", "nome")
  ```

  **Nada é executado ainda.**
  O Spark só vai **montando um plano de execução** (um grafo de operações).

#### ✅ Actions (disparam a execução)

Ex.: `show()`, `count()`, `collect()`, `write`, `save`, etc.

* Quando se faz:

  ```python
  df3.show()
  ```

  O Spark:

  * lê os dados,
  * aplica `filter`,
  * aplica `select`,
  * e mostra o resultado.

---

#### __3.2.2. Por que o Spark faz isso?__

Porque o lazy evaluation permite:

1. **Otimização do plano completo**

   * Ele vê **toda a sequência** de passos antes de rodar.
   * Aí pode:

     * empurrar `filter` o mais cedo possível (menos dados para processar),
     * combinar operações,
     * evitar leituras/movimentações desnecessárias.

2. **Menos leituras e menos custo**

   * QUando são feitas várias transformations e nunca chama uma action,
     o Spark **não gasta recurso à toa**.

3. **Tolerância a falha**

   * Ele guarda o *lineage* (histórico de transformations).
   * Se algo falha, consegue **recalcular uma parte** a partir da origem.

---

#### __3.2.3. Exemplo em PySpark__

```python
# nada disso executa de imediato
df = spark.read.parquet("s3://meu-bucket/dados")
df_filtrado = df.filter(df.ano == 2024)
df_agregado = df_filtrado.groupBy("estado").count()

# aqui o Spark realmente executa o plano todo
df_agregado.show()
```

* O processamento **só acontece** quando chamamos `show()`.

#### __3.3 Particionamento e Bucketing__
- Os dados são particionados por padrão.
- Podem ser particionados explicitamemte em disco (partitioned by) ou em memória - repartition() ou coalesce().
- O bucketing é semelhante a ao particionamento, mas com númerofixo de particções.
- O bucketing é ideal para colunas com alta cardinalidade.
- Particionamento e Bucketing podem ser usados em conjunto.

#### __3.3 Arquitetura e componetes do Spark__
- Componentes: <br> - Machine Learning (Mlib). <br> - SQL (Spark SQL). <br> - Processamento em Streaming. <br> - Processameno de grafos.
- Estrutura: <br> - Driver: inicializa SparkSession; solcota recursos computacionais de cluster manager; transforma as operações em DAGs e as distribui pelos executers. <br> - Manager: gerencia os recusros de cluster (built-in standalone; YARN; Mesos; Kubernetes).<br> - Executers: roda em cada nó do cluster, executando as tarefas.

#### __3.4 Spark Context e Session__
Boa, essa é bem importante pra prova e pro dia a dia.

#### __3.4.1. SparkContext (sc)__

O **SparkContext** é o *ponto de entrada de baixo nível* do Spark, responsável por:

* Conectar sua aplicação ao **cluster** (ou ao modo local).
* Gerenciar os **executors** e recursos (CPU, memória).
* Enviar tarefas para o cluster.
* Trabalhar com a API de **RDDs** (Resilient Distributed Datasets).

Historicamente, o código começava assim (Scala/Python antigos):

```python
from pyspark import SparkConf, SparkContext

conf = SparkConf().setAppName("MinhaApp").setMaster("local[*]")
sc = SparkContext(conf=conf)
```

O `sc` era utilizado para:

```python
rdd = sc.textFile("meu_arquivo.txt")
```

Atualmente quase não se cria `SparkContext` “na mão” — ele fica escondido dentro da `SparkSession`.

---

#### __3.4.2. SparkSession (spark)__

A **SparkSession** é o *ponto de entrada unificado* do Spark (desde a versão 2.0):

> É o objeto principal para trabalhar com **DataFrame**, **Spark SQL**, **Structured Streaming**, catálogo de tabelas etc.

Exemplo típico em PySpark:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MinhaApp") \
    .getOrCreate()
```

Com a `spark` você faz:

```python
df = spark.read.parquet("meus_dados.parquet")
df.createOrReplaceTempView("minha_tabela")
spark.sql("SELECT * FROM minha_tabela WHERE ano = 2024").show()
```

---

#### __3.4.3. Relação entre SparkSession e SparkContext__

* A `SparkSession` **encapsula** um `SparkContext`.
* O acesso é efetuado da seguinte forma:

  ```python
  sc = spark.sparkContext
  ```

Resumindo:

* **SparkContext** = engine de baixo nível (conexão com o cluster, RDDs).
* **SparkSession** = camada de alto nível, usada no dia a dia (DataFrames, SQL, streaming), que *usa* o SparkContext por baixo dos panos.

* `SparkContext`

  * Ponto de entrada **antigo** e de **baixo nível**.
  * Conecta ao cluster e gerencia execução.
  * Trabalha com **RDD**.

* `SparkSession`

  * Ponto de entrada **moderno e recomendado**.
  * Unifica: SQL + DataFrame + Streaming + catálogo.
  * Internamente contém um `SparkContext`.


### __4. Conhecendo o Databricks__ 
### __5. Data Frames, Delta, Delta Lake__
### __6. Gráficos e Dashboards__
### __7. Pyspark.pandas__ 
### __8. Construindo um Data Lakehouse__
### __9. Conectando ao Databricks__
