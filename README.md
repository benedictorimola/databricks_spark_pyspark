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
### __4. Conhecendo o Databricks__ 
### __5. Data Frames, Delta, Delta Lake__
### __6. Gráficos e Dashboards__
### __7. Pyspark.pandas__ 
### __8. Construindo um Data Lakehouse__
### __9. Conectando ao Databricks__
