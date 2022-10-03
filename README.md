## 🎓 Spring, Quarkus and Micronaut with Apache Cassandra

<img src="images/badge.png?raw=true" width="150" align="right" />

[![License Apache2](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Discord](https://img.shields.io/discord/685554030159593522)](https://discord.com/widget?id=685554030159593522&theme=dark)

Welcome to the *Explore SpringBoot, Quarkus and Micronaut microservices with NoSQL Apache Cassandra** workshop! In this two-hour workshop, we will show you a sample architecture making use of Apache Cassandra™ with the three leading implementation of Java platform

⏲️ **Duration :** 2 hours

🎓 **Level** Beginner to Intermediate

![](images/splash.png)

> [🔖 Accessing HANDS-ON](#-start-hands-on)

## 📋 Table of contents

- **HouseKeeping**
  - [Objectives](#objectives)
  - [Frequently asked questions](#frequently-asked-questions)
  - [Materials for the Session](#materials-for-the-session)

- [Setup your environment (DB, IDE)](#setup---initialize-your-environment)
- [LAB1 - Understanding java drivers](#lab1---producer-and-consumer)
- [LAB2 - Spring Boot](#lab2---pulsar-functions)
- [LAB3 - Quarkus](#lab3---working-with-databases)
- [LAB4 - Micronaut](#lab4---pulsar-io)
- [Homework](#Homework)
<p>

## Objectives

- 🎯 Give you an understanding and how and where to position Apache Pulsar

- 🎯 Give an overview of  streaming and datascience ecosystem**

- 🎯 Give you an understanding of Apache Cassandra NoSQL Database

- 🎯 Create your first pipeline with streaming and database.

- 🚀 Have fun with an interactive session

## Frequently asked questions

<p/>
<details>
<summary><b> 1️⃣ Can I run this workshop on my computer?</b></summary>
<hr>
<p>There is nothing preventing you from running the workshop on your own machine. If you do so, you will need the following:
<ol>
<li><b>git</b>
<li><b>Python 3.6+</b>
<li><b>Astra Cli</b>
<li><b>Pulsar Shell or Pulsar-Client</b>
</ol>
</p>
In this readme, we try to provide instructions for local development as well - but keep in mind that the main focus is development on Gitpod, hence <strong>we can't guarantee live support</strong> about local development in order to keep on track with the schedule. However, we will do our best to give you the info you need to succeed.
</details>
<p/>
<details>
<summary><b> 2️⃣ What other prerequisites are required?</b></summary>
<hr>
<ul>
<li>You will need enough *real estate* on screen, we will ask you to open a few windows and it would not fit on mobiles (tablets should be OK)
<li>You will need an Astra account: don't worry, we'll work through that in the following
<li>As "Intermediate level" we expect you to know what java and Spring are. 
</ul>
</p>
</details>
<p/>
<details>
<summary><b> 3️⃣ Do I need to pay for anything for this workshop?</b></summary>
<hr>
<b>No.</b> All tools and services we provide here are FREE. FREE not only during the session but also after.
</details>
<p/>
<details>
<summary><b> 4️⃣ Will I get a certificate if I attend this workshop?</b></summary>
<hr>
Attending the session is not enough. You need to complete the homework detailed below and you will get a nice badge that you can share on linkedin or anywhere else *(open badge specification)*
</details>
<p/>

## Materials for the session

It doesn't matter if you join our workshop live or you prefer to work at your own pace,
we have you covered. In this repository, you'll find everything you need for this workshop:

- [Slide deck](/slides/slides.pdf)
- [Discord chat](https://dtsx.io/discord)
- [Questions and Answers](https://community.datastax.com/)
- [Twitch backup](https://www.twitch.tv/datastaxdevs)

# 🏁 Start Hands-on

## Setup

#### `✅.setup-01`- Create your [Astra Account](https://astra.dev/yt-10-5)

> *ℹ️ Documentation:[Database creation guide](https://awesome-astra.github.io/docs/pages/astra/create-instance/#c-procedure)*

#### `✅.setup-02`- Create `Database Asministrator` Token.

>  *ℹ️ Documentation: [Token creation guide](https://awesome-astra.github.io/docs/pages/astra/create-token/#c-procedure)*.

```
Skip this step is you already have a token. You can reuse the same token in our other workshops, too. Your token should look like: `AstraCS:....`
```


|Field|Value|
|---|---|
|**Role**| `Database Administrator`|


![token](https://awesome-astra.github.io/docs/img/astra/astra-create-token.gif)


#### `✅.setup-03`- Open Gitpod

Gitpod is an IDE based on VSCode deployed in the cloud.

> ↗️ _Right Click and select open as a new Tab..._

<a href="https://gitpod.io/#https://github.com/datastaxdevs/workshop-spring-quarkus-micronaut-cassandra"><img src="https://dabuttonfactory.com/button.png?t=Open+Gitpod&f=Open+Sans-Bold&ts=16&tc=fff&hp=20&vp=10&c=11&bgt=unicolored&bgc=0b5394" /></a>


#### `✅.setup-04`- Setup Astra CLI

Go back to your gitpod terminal waiting for your token. Make sure you select the `database` shell in the bottom-right panel and provide the value where it is asked.


> 🖥️ `setup-04 output`
>
> ```
> [cedrick.lunven@gmail.com]
> ASTRA_DB_APPLICATION_TOKEN=AstraCS:AAAAAAAA
> 
> [What's NEXT ?]
> You are all set.(configuration is stored in ~/.astrarc) You can now:
>    • Use any command, 'astra help' will get you the list
>    • Try with 'astra db list'
>    • Enter interactive mode using 'astra'
> 
> Happy Coding !
> ```


#### `✅.setup-05`- List your existing Users.

```bash
astra user list
```

> 🖥️ `setup-05 output`
>
> ```
> +--------------------------------------+-----------------------------+---------------------+
> | User Id                              | User Email                  | Status              |
> +--------------------------------------+-----------------------------+---------------------+
> | b665658a-ae6a-4f30-a740-2342a7fb469c | cedrick.lunven@datastax.com | active              |
> +--------------------------------------+-----------------------------+---------------------+
> ```

#### `✅.setup-06`- Create database `workshops` and keyspace `ks_java` if they do not exist:

```bash
astra db create workshops -k ks_java --if-not-exist --wait
```

Let's analyze the command:
| Chunk         | Description     |
|--------------|-----------|
| `db create` | Operation executed `create` in group `db`  |
| `workshops` | Name of the database, our argument |
|`-k ks_java` | Name of the keyspace, a db can contains multiple keyspaces |
| `--if-not-exist` | Flag for itempotency creating only what if needed |
| `--wait` | Make the command blocking until all expected operations are executed (timeout is 180s) |

> **Note**: If the database already exist but has not been used for while the status will be `HIBERNATED`. The previous command will resume the db an create the new keyspace but it can take about a minute to execute.

> 🖥️ `setup-06 output`
>
> ```
> [ INFO ] - Database 'workshops' already exist. Connecting to database.
> [ INFO ] - Database 'workshops' has status 'MAINTENANCE' waiting to be 'ACTIVE' ...
>[ INFO ] - Database 'workshops' has status 'ACTIVE' (took 7983 millis)
> ```

#### `✅.setup-07`- Check the status of database `workshops`

```bash
astra db status workshops
```

> 🖥️ `setup-07 output`
>
> ```
> [ INFO ] - Database 'workshops' has status 'ACTIVE'
> ```



*Congratulations your environment is all set, let's start the labs !*

# LAB 1 - Understanding Java Drivers

## 1.1 - Configuration

#### `✅.1.1.a`- Get the informations for your database including the keyspace list

```bash
astra db get workshops
```

> 🖥️ `output`
>
> ```
> +------------------------+-----------------------------------------+
> | Attribute              | Value                                   |
> +------------------------+-----------------------------------------+
> | Name                   | workshops                               |
> | id                     | bb61cfd6-2702-4b19-97b6-3b89a04c9be7    |
> | Status                 | ACTIVE                                  |
> | Default Cloud Provider | AWS                                     |
> | Default Region         | us-east-1                               |
> | Default Keyspace       | ks_java                                 |
> | Creation Time          | 2022-08-29T06:13:06Z                    |
> |                        |                                         |
> | Keyspaces              | [0] ks_java                             |
> |                        |                                         |
> |                        |                                         |
> | Regions                | [0] us-east-1                           |
> |                        |                                         |
> +------------------------+-----------------------------------------+
> ```

#### `✅.1.1.b`- Download Secure bundle

```
astra db download-scb workshops -f /workspace/workshop-spring-quarkus-micronaut-cassandra/secure-bundle-workshops.zip
```

```
ls -l /workspace/workshop-spring-quarkus-micronaut-cassandra/
```

#### `✅.1.1.c`- Register token as env variable

ASTRA_DB_APP_TOKEN=`astra config get default --key ASTRA_DB_APPLICATION_TOKEN`

# LAB 2 - Spring Data Cassandra

## 2.1 - Configuration

#### `✅.2.1.a`- Create keyspace `ks_spring`

```bash
astra db create-keyspace workshops -k ks_spring
```

#### `✅.2.1.a`- list Keyspaces

```bash
astra db list-keyspaces workshops
```

> 🖥️ `output`
>
> ```
> +---------------------+
> | Name                |
> +---------------------+
> | ks_spring           |
> | ks_java (default)   |
> +---------------------+

#### 📘 Ce qu'il faut retenir:

- [Spring Data](https://spring.io/projects/spring-data) est la couche d'accès aux données proposée dans le framework spring. Elle se décline pour plusieurs bases de données à la fois SQL (JPA) et NoSQL (Cassandra, Mongo, Redis...)

- [Spring Data Cassandra](https://spring.io/projects/spring-data-cassandra) comporte 1 librairie [`spring-data-cassandra`](https://mvnrepository.com/artifact/org.springframework.data/spring-data-cassandra) et la dernière version est [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss/java-driver-core/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.springframework.data/spring-data-cassandra)

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-cassandra</artifactId>
    <version>${latest}</version>
</dependency>
```

- Depuis les versions `3.x` Spring Data s'appuie sur la dernière génération de drivers Cassandra `4.x`. Dans nos exemples nous allons nous appuyer sur `Spring-boot`. Pour utiliser la dernière génération nous devons utiliser une version supérieure a `2.3+`. Les compatibilités sont décrites dans le tableau ci-dessous:

> | Drivers       | Spring-Data    | Spring Boot    |
> | ------------- | -------------- | -------------- |
> | Drivers `3.x` | `2.2` et avant | `2.2` et avant |
> | Drivers `4.x` | `3.x` et après | `2.3` et avant |

- Pour utiliser `Spring Data Cassandra` avec `Spring Boot` il existe 2 starters différents `spring-boot-starter-data-cassandra` (MVC) et `spring-boot-starter-data-cassandra-reactive` (Webflux). Dans notre exmple nous utilisons la première mais un exemple réactif est [disponible ici](https://github.com/datastaxdevs/workshop-spring-reactive)

#### `✅.131`- Vérifier le `pom.xml`

- Ouvrir le fichier

```bash
gp open /workspace/conference-2022-devoxx/labs/lab5_spring_data/pom.xml
```

- Vous devez retrouver:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-cassandra</artifactId>
</dependency>
```

#### `✅.132`- Configuration de l'application Spring-Data

- Repérer le terminal `lab5_spring_data` et compiler le projet

```bash
cd /workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn clean compile
```

- Localiser le fichier de configuration `application.yml`dans le répertoire `src/main/resources`. C'est le fichier de configuration principal de Spring-Boot.

```bash
gp open /workspace/conference-2022-devoxx/labs/lab5_spring_data/src/main/resources/application.yml
```

- Suivant la cible (Cassandra dans Docker ou Cassandra dans Astra) la configuration de `spring-data` changera légèrement c'est pourquoi nous avons proposé 2 exemple `application-astra.yml` et `application-astra.yml`

- Copier le fichier qui vous correspond vers `application.yml`

```bash
cp /workspace/conference-2022-devoxx/labs/lab5_spring_data/src/main/resources/application-astra.yml /workspace/conference-2022-devoxx/labs/lab5_spring_data/src/main/resources/application.yml
```

ou

```bash
cp cp/workspace/conference-2022-devoxx/labs/lab5_spring_data/src/main/resources/application-local.yml /workspace/conference-2022-devoxx/labs/lab5_spring_data/src/main/resources/application.yml
```

- Vérifier la configuration et éditer là le cas échéant:

_application-astra.yml_

```yaml
spring:
  data:
    cassandra:
      schema-action: CREATE_IF_NOT_EXISTS
      keyspace-name: devoxx_spring
      username: token
      password: AstraCS:<votre_jeton>
datastax:
  astra:
    secure-connect-bundle: /home/gitpod/.cassandra/bootstrap.zip
```

_application-local.yml_

```yaml
spring:
  data:
    cassandra:
      schema-action: CREATE_IF_NOT_EXISTS
      keyspace-name: devoxx_spring
      contact-points: localhost:9042
      local-datacenter: dc1
```

#### `✅.133`- Validation de la configuration

```bash
/workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn test -Dtest=com.datastax.workshop.E01_SpringDataInit
```

#### 🖥️ Logs

```
[INFO] Running com.datastax.workshop.E01_SpringDataInit
 ________                                  _______________   ________ ________
 \______ \   _______  _________  ______  __\_____  \   _  \  \_____  \\_____  \
 |    |  \_/ __ \  \/ /  _ \  \/  /\  \/  //  ____/  /_\  \  /  ____/ /  ____/
 |    `   \  ___/\   (  <_> >    <  >    </       \  \_/   \/       \/       \
 /_______  /\___  >\_/ \____/__/\_ \/__/\_ \_______ \_____  /\_______ \_______ \
 \/     \/                \/      \/       \/     \/         \/       \/

 The application will start at http://localhost:8080

13:49:30.253 INFO  com.datastax.workshop.E01_SpringDataInit      : Starting E01_SpringDataInit using Java 17.0.1 on clunven-rmbp16 with PID 33320 (started by cedricklunven in /Users/cedricklunven/dev/workspaces/datastax/conference-2022-devoxx/labs/2-spring-data)
13:49:30.255 INFO  com.datastax.workshop.E01_SpringDataInit      : No active profile set, falling back to default profiles: default
13:49:34.035 INFO  com.datastax.workshop.E01_SpringDataInit      : Started E01_SpringDataInit in 3.965 seconds (JVM running for 4.659)
13:49:34.329 INFO  com.datastax.workshop.E01_SpringDataInit      : Creating your CqlSession to Cassandra...
13:49:34.329 INFO  com.datastax.workshop.E01_SpringDataInit      : + [OK] Your are connected to keyspace devoxx_spring
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.604 s - in com.datastax.workshop.E01_SpringDataInit
[INFO]
```

## 5.2 - Comprendre les `CrudRepositories`

#### 📘 Ce qu'il faut retenir:

- Spring Data propose une system d'objet mapping pour associer les objets aux tables du modèles de données. Il utilise une interface générique `CrudRepository`.

- Travaillons avec le modèle (non optimisé) d'une todolist.

```sql
CREATE TABLE todos (
    uid uuid PRIMARY KEY,
    completed boolean,
    offset int,
    title text
)
```

- On définit un objet `TodoEntity` et on l'annote avec les annotations Spring Data.

> ```java
> @Table(value = TodoEntity.TABLENAME)
> public class TodoEntity {
>
>  public static final String TABLENAME        = "todos";
>  public static final String COLUMN_UID       = "uid";
>  public static final String COLUMN_TITLE     = "title";
>  public static final String COLUMN_COMPLETED = "completed";
>  public static final String COLUMN_ORDER     = "offset";
>
>  @PrimaryKey
>  @Column(COLUMN_UID)
>  @CassandraType(type = Name.UUID)
>  private UUID uid;
>
>  @Column(COLUMN_TITLE)
>  @CassandraType(type = Name.TEXT)
>  private String title;
>
>  @Column(COLUMN_COMPLETED)
>  @CassandraType(type = Name.BOOLEAN)
>  private boolean completed = false;
>
>  @Column(COLUMN_ORDER)
>  @CassandraType(type = Name.INT)
>  private int order = 0;
>
>  public TodoEntity(String title, int offset) {
>    this(UUID.randomUUID(), title, false, offset);
>  }
> }
> ```

- On définit une interface qui hérite de `CassandraRepository` (elle-même hérite de `CRUDRepository`) en spécifiant le bean et la clé primaire.

```java
@Repository
public interface TodoRepositoryCassandra extends CassandraRepository<TodoEntity, UUID> {
}
```

#### `✅.134`- Utiliser les `Repository` Spring Data

```bash
cd /workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn test -Dtest=com.datastax.workshop.E02_SpringDataRepository
```

#### 🖥️ Logs

```bash
[INFO] Running com.datastax.workshop.E02_SpringDataRepository
 ________                                  _______________   ________ ________
 \______ \   _______  _________  ______  __\_____  \   _  \  \_____  \\_____  \
 |    |  \_/ __ \  \/ /  _ \  \/  /\  \/  //  ____/  /_\  \  /  ____/ /  ____/
 |    `   \  ___/\   (  <_> >    <  >    </       \  \_/   \/       \/       \
 /_______  /\___  >\_/ \____/__/\_ \/__/\_ \_______ \_____  /\_______ \_______ \
 \/     \/                \/      \/       \/     \/         \/       \/

 The application will start at http://localhost:8080

14:06:54.529 INFO  com.datastax.workshop.E02_SpringDataRepository : Starting E02_SpringDataRepository using Java 17.0.1 on clunven-rmbp16 with PID 33643 (started by cedricklunven in /Users/cedricklunven/dev/workspaces/datastax/conference-2022-devoxx/labs/2-spring-data)
14:06:54.530 INFO  com.datastax.workshop.E02_SpringDataRepository : No active profile set, falling back to default profiles: default
14:06:58.212 INFO  com.datastax.workshop.E02_SpringDataRepository : Started E02_SpringDataRepository in 3.895 seconds (JVM running for 4.565)
14:06:58.635 INFO  com.datastax.workshop.E02_SpringDataRepository : Tache enregistree avec id 8a175b9e-1010-4f9a-aa5c-628c81c8dd34
14:06:58.636 INFO  com.datastax.workshop.E02_SpringDataRepository : Liste des Taches
14:06:58.746 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=8a175b9e-1010-4f9a-aa5c-628c81c8dd34, title=Apprendre Cassandra, completed=false, order=0)
14:06:58.746 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=87eb778d-a938-441e-8ff5-e69feafb8719, title=Apprendre Cassandra, completed=false, order=0)
14:06:58.746 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=47a5c298-b6ec-4e8a-abb5-fca041730af3, title=Apprendre Cassandra, completed=false, order=0)
14:06:58.746 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=3847d7f9-0fa3-4d7e-b7f7-b76897b4e999, title=Apprendre Cassandra, completed=false, order=0)
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.724 s - in com.datastax.workshop.E02_SpringDataRepository
```

#### `✅.135`- Vérifier le résultat avec `CQLSh`

```sql
use devoxx_spring;
SELECT * FROM todos;
```

#### 🖥️ Logs

```bash
token@cqlsh:devoxx_spring> SELECT * FROM todos;

 uid                                  | completed | offset | title
--------------------------------------+-----------+--------+---------------------
 8a175b9e-1010-4f9a-aa5c-628c81c8dd34 |     False |      0 | Apprendre Cassandra
 87eb778d-a938-441e-8ff5-e69feafb8719 |     False |      0 | Apprendre Cassandra
 47a5c298-b6ec-4e8a-abb5-fca041730af3 |     False |      0 | Apprendre Cassandra
 3847d7f9-0fa3-4d7e-b7f7-b76897b4e999 |     False |      0 | Apprendre Cassandra

(4 rows)
```

## 5.3 - CassandraOperations

#### 📘 Ce qu'il faut retenir:

- Les `Repository` sont très puissants mais ne permettent pas tout. Le risque est de chercher à réutiliser les mêmes beans et les mêmes repositories pour différentes requêtes sur la même données alors que vous devez définir plusieurs tables.

- Spring Data propose l'accès aux opérations `CqlSession` sous-jacente au travers de objets `CassandraOperations` et `CassandraTemple`. Vous pouvez les injecter lorsque vous en avez besoin. Ils sont également disponibles dans les repository si vous héritez de `SimpleCassandraRepository`.

```java
@Repository
public class TodoRepositorySimpleCassandra extends SimpleCassandraRepository<TodoEntity, UUID> {

 protected final CqlSession cqlSession;

 protected final CassandraOperations cassandraTemplate;

 @SuppressWarnings("unchecked")
 public TodoRepositorySimpleCassandra(CqlSession cqlSession, CassandraOperations ops) {
   super(new MappingCassandraEntityInformation<TodoEntity, UUID>(
     (CassandraPersistentEntity<TodoEntity>) ops.getConverter().getMappingContext()
     .getRequiredPersistentEntity(TodoEntity.class), ops.getConverter()), ops);
   this.cqlSession = cqlSession;
   this.cassandraTemplate = ops;
 }
}
```

- L'objet `CqlSession` fait partie du contexte Spring et vous pouvez également l'utiliser au besoin.

#### `✅.136`- Utiliser `CassandraOperations` et un `SimpleCassandraRepository`

```bash
cd /workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn test -Dtest=com.datastax.workshop.E03_SpringDataCassandraOperations
```

#### 🖥️ Logs

```bash
[INFO] Running com.datastax.workshop.E03_SpringDataCassandraOperations
 ________                                  _______________   ________ ________
 \______ \   _______  _________  ______  __\_____  \   _  \  \_____  \\_____  \
 |    |  \_/ __ \  \/ /  _ \  \/  /\  \/  //  ____/  /_\  \  /  ____/ /  ____/
 |    `   \  ___/\   (  <_> >    <  >    </       \  \_/   \/       \/       \
 /_______  /\___  >\_/ \____/__/\_ \/__/\_ \_______ \_____  /\_______ \_______ \
 \/     \/                \/      \/       \/     \/         \/       \/

 The application will start at http://localhost:8080

14:22:16.841 INFO  com.datastax.workshop.E03_SpringDataCassandraOperations : Starting E03_SpringDataCassandraOperations using Java 17.0.1 on clunven-rmbp16 with PID 33920 (started by cedricklunven in /Users/cedricklunven/dev/workspaces/datastax/conference-2022-devoxx/labs/2-spring-data)
14:22:16.843 INFO  com.datastax.workshop.E03_SpringDataCassandraOperations : No active profile set, falling back to default profiles: default
14:22:20.384 INFO  com.datastax.workshop.E03_SpringDataCassandraOperations : Started E03_SpringDataCassandraOperations in 3.755 seconds (JVM running for 4.457)
14:22:20.768 INFO  com.datastax.workshop.E02_SpringDataRepository : Tache enregistree avec id e73dcd8f-4427-42ab-9e32-4db8fd1a1144
14:22:20.769 INFO  com.datastax.workshop.E02_SpringDataRepository : Liste des Taches
14:22:20.865 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=8a175b9e-1010-4f9a-aa5c-628c81c8dd34, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.865 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=e73dcd8f-4427-42ab-9e32-4db8fd1a1144, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.865 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=87eb778d-a938-441e-8ff5-e69feafb8719, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.865 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=47a5c298-b6ec-4e8a-abb5-fca041730af3, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.865 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=3847d7f9-0fa3-4d7e-b7f7-b76897b4e999, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.875 INFO  com.datastax.workshop.E02_SpringDataRepository : Utilisation de CassandraOperations
14:22:20.984 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=8a175b9e-1010-4f9a-aa5c-628c81c8dd34, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.984 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=e73dcd8f-4427-42ab-9e32-4db8fd1a1144, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.984 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=87eb778d-a938-441e-8ff5-e69feafb8719, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.984 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=47a5c298-b6ec-4e8a-abb5-fca041730af3, title=Apprendre Cassandra, completed=false, order=0)
14:22:20.984 INFO  com.datastax.workshop.E02_SpringDataRepository : TodoEntity(uid=3847d7f9-0fa3-4d7e-b7f7-b76897b4e999, title=Apprendre Cassandra, completed=false, order=0)
[INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.677 s - in com.datastax.workshop.E03_SpringDataCassandraOperations
```

## 5.4 - Application Spring Boot

#### 📘 Ce qu'il faut retenir:

- Les différents `Repository` peuvent être injectés dans les controllers et exposés au niveau des APIs.

![](img/spring_layers.png?raw=true)

Une bonne pratique est de séparer les objets utilisés dans la couche d'accès aux données (entités) des objets utilisés dans les Apis (DTO).

#### `✅.137`- Lancer l'application

- Démarrer l'application à l'aide du plugin `spring-boot`

```bash
cd /workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn spring-boot:run
```

- L'application démarre sur le port `8080`. La liste des `todos` est disponible sur `http://localhost:8080/api/v1/todos/`. Sur gitpod les ports n'étant pas ouverts il y a aura une translation d'adresse. Afficher l'Url gitpod

![](img/spring_api_local.png?raw=true)

- Afficher l'url translatée par `Gitpod` _(`gp` est la ligne de commande de gitpod)_

```bash
gp url 8080
```

- Afficher la liste des `todos`

```
gp preview "$(gp url 8080)/api/v1/todos/"
```

![](img/spring_api_gitpod.png?raw=true)

#### `✅.138`- Tests d'intégration de l'application

- Stopper l'application avec un `CTRL+C`

- Editer la classe `E04_SpringControllerTest` pour remplacer `createURLWithPort` avec l'url de votre gitpod :

_de:_

```java
private String createURLWithPort(String uri) {
  return "http://localhost:" + port + uri;
}
```

_à (ici 8080-datastaxdevs-conference2-g3jf9fgchk4.ws-eu34.gitpod.io est le résultat de ma commande gp url 8080):_

```java
private String createURLWithPort(String uri) {
  return "https://8080-datastaxdevs-conference2-g3jf9fgchk4.ws-eu34.gitpod.io" + uri;
}
```

- Exécuter le test unitaire suivant:

```bash
cd /workspace/conference-2022-devoxx/labs/lab5_spring_data
mvn test -Dtest=com.datastax.workshop.E04_SpringControllerTest
```

#### 🖥️ Logs

```bash
[INFO] Running com.datastax.workshop.E04_SpringControllerTest
 ________                                  _______________   ________ ________
 \______ \   _______  _________  ______  __\_____  \   _  \  \_____  \\_____  \
 |    |  \_/ __ \  \/ /  _ \  \/  /\  \/  //  ____/  /_\  \  /  ____/ /  ____/
 |    `   \  ___/\   (  <_> >    <  >    </       \  \_/   \/       \/       \
 /_______  /\___  >\_/ \____/__/\_ \/__/\_ \_______ \_____  /\_______ \_______ \
 \/     \/                \/      \/       \/     \/         \/       \/

 The application will start at http://localhost:8080

15:41:30.731 INFO  com.datastax.workshop.E04_SpringControllerTest : Starting E04_SpringControllerTest using Java 17.0.1 on clunven-rmbp16 with PID 41891 (started by cedricklunven in /Users/cedricklunven/dev/workspaces/datastax/conference-2022-devoxx/labs/2-spring-data)
15:41:30.733 INFO  com.datastax.workshop.E04_SpringControllerTest : No active profile set, falling back to default profiles: default
15:41:34.436 INFO  com.datastax.workshop.E04_SpringControllerTest : Started E04_SpringControllerTest in 3.898 seconds (JVM running for 4.712)
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.918 s - in com.datastax.workshop.E04_SpringControllerTest
```

<p/><br/>

> [🏠 Retour à la table des matières](#-table-des-matières)

# LAB 6 - Cassandra Quarkus Extension

## 6.1 - Introduction aux extensions Quarkus

#### 📘 Ce qu'il faut retenir:

[Quarkus](https://quarkus.io/) est un framework pour construire des microservices sur la plateforme Java. Le parti pris est de réaliser le plus d'opérations possibles durant le build et de ne packager que ce qui est absolument nécessaire. Les objectifs sont:

- La production d'une image native de quelques mega-octets seulement
- La production d'une image qui démarre en quelques millièmes de seconde.

Une [extension Quarkus](https://quarkus.io/guides/writing-extensions) permet de simplifier la configuration d'une application et d'assurer une meilleure compatibilité. L'équipe `Datastax` a créé et open sourcé une extension pour Cassandra [ici](https://github.com/datastax/cassandra-quarkus). Voici ce qu'elle permet:

- Le support de réactif avec `Mutiny` (couche réactive de Quarkus)
- L'intégration avec `vertx` et le event loop de Quarkus
- Déclarer les `Mapper` (object mapping) dans `Arc`, le système d'injection de dépendances de Quarkus.
- Fournir des hints pour la création d'une native image _aux petits oignons._

La librairie à utiliser est `cassandra-quarkus-client` et la version est [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss.quarkus/cassandra-quarkus-client/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss.quarkus/cassandra-quarkus-client)

```xml
<dependency>
  <groupId>com.datastax.oss.quarkus</groupId>
  <artifactId>cassandra-quarkus-client</artifactId>
  <version>${latest}</version>
</dependency>
```

Quarkus propose également un guide très bien fait sur le support de Cassandra [ici](https://quarkus.io/guides/cassandra)

## 6.2 - Connexion et configuration

#### `✅.139`- Création du keyspace `devoxx_quarkus`

_Dans Docker:_

```sql
CREATE KEYSPACE IF NOT EXISTS devoxx_quarkus
WITH REPLICATION = {
  'class' : 'NetworkTopologyStrategy',
  'dc1' : 3
}  AND DURABLE_WRITES = true;
```

Avec Astra, la manipulation des keyspaces est désactivée, c'est lui qui fixe les facteurs de réplications pour vous (Saas). La procédure est décrite en détail dans [Awesome Astra](https://awesome-astra.github.io/docs/pages/astra/faq/#how-do-i-create-a-namespace-or-a-keyspace) mais voici quelques captures:

_Repérer le bouton `ADD KEYSPACE`_
![](https://awesome-astra.github.io/docs/img/faq/create-keyspace-button.png)

_Créer le keyspace `devoxx_quarkus` et valider avec `SAVE`_
![](https://awesome-astra.github.io/docs/img/faq/create-keyspace.png)

#### `✅.140`- Configuration de l'application `Quarkus`

- Placer vous dans le répertoire `lab6_quarkus` et compiler le projet

```bash
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn clean compile
```

- Localiser le fichier de configuration `application.properties` dans le répertoire `src/main/resources`. C'est le fichier de configuration principal de Quarkus. Noter le nombre de clés de configuration `quarkus.cassandra`

```bash
gp open /workspace/conference-2022-devoxx/labs/lab6_quarkus/src/main/resources/application.properties
```

- Suivant la cible (Cassandra dans Docker ou Cassandra dans Astra) la configuration de `quarkus` changera légèrement c'est pourquoi nous avons proposé 2 exemples `application-astra.properties` et `application-local.properties`

- Copier le fichier qui vous correspond vers `application.properties`

```bash
cp /workspace/conference-2022-devoxx/labs/lab6_quarkus/src/main/resources/application-astra.properties /workspace/conference-2022-devoxx/labs/lab6_quarkus/src/main/resources/application.properties
```

ou

```bash
cp /workspace/conference-2022-devoxx/labs/lab6_quarkus/src/main/resources/application-local.properties /workspace/conference-2022-devoxx/labs/lab6_quarkus/src/main/resources/application.propertoes
```

- Dans le cas de Astra changer la clef `quarkus.cassandra.auth.password` pour correspondre à votre base.

```ini
quarkus.cassandra.keyspace=devoxx_quarkus
quarkus.cassandra.cloud.secure-connect-bundle=/home/gitpod/.cassandra/bootstrap.zip
quarkus.cassandra.auth.username=<client_id>
quarkus.cassandra.auth.password=<client_secret>
```

#### `✅.141` - Validation de la configuration

```
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn test -Dtest=com.datastax.workshop.E01_QuarkusInit
```

#### 🖥️ Logs

```
[INFO] Running com.datastax.workshop.E01_QuarkusInit
2022-04-19 19:18:06,628 INFO  [io.qua.arc.pro.BeanProcessor] (build-15) Found unrecommended usage of private members (use package-private instead) in application beans:
	- @Inject field com.datastaxdev.todo.TodoRestController#cqlSession,
	- @Inject field com.datastaxdev.todo.TodoRestController#uriInfo
2022-04-19 19:18:06,651 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-29) Micrometer metrics were enabled by configuration, but MicrometerMetricsFactory was not found.
2022-04-19 19:18:06,651 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-29) Make sure to include a dependency to the java-driver-metrics-micrometer module.
2022-04-19 19:18:06,952 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.13.0
2022-04-19 19:18:08,100 INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
2022-04-19 19:18:08,856 INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-8) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
2022-04-19 19:18:09,215 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
2022-04-19 19:18:09,255 INFO  [io.quarkus] (main) Quarkus 2.3.1.Final on JVM started in 3.793s. Listening on: http://localhost:8081
2022-04-19 19:18:09,255 INFO  [io.quarkus] (main) Profile test activated.
2022-04-19 19:18:09,255 INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
2022-04-19 19:18:09,619 INFO  [com.dat.wor.E01_QuarkusInit] (main) Creating your CqlSession to Cassandra...
2022-04-19 19:18:09,621 INFO  [com.dat.wor.E01_QuarkusInit] (main) + [OK] Your are connected to keyspace devoxx_quarkus
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.518 s - in com.datastax.workshop.E01_QuarkusInit
2022-04-19 19:18:09,641 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Closing Quarkus Cassandra session.
2022-04-19 19:18:09,657 INFO  [io.quarkus] (main) Quarkus stopped in 0.021s
```

#### `✅.142` - Utilisation de `CqlSession` avec `Quarkus`

```
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn test -Dtest=com.datastax.workshop.E02_QuarkusCql
```

#### 🖥️ Logs

```bash
[INFO] Running com.datastax.workshop.E02_QuarkusCql
2022-04-19 19:21:07,918 INFO  [io.qua.arc.pro.BeanProcessor] (build-20) Found unrecommended usage of private members (use package-private instead) in application beans:
	- @Inject field com.datastaxdev.todo.TodoRestController#cqlSession,
	- @Inject field com.datastaxdev.todo.TodoRestController#uriInfo
2022-04-19 19:21:07,942 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-5) Micrometer metrics were enabled by configuration, but MicrometerMetricsFactory was not found.
2022-04-19 19:21:07,943 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-5) Make sure to include a dependency to the java-driver-metrics-micrometer module.
2022-04-19 19:21:08,289 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.13.0
2022-04-19 19:21:09,543 INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
2022-04-19 19:21:10,202 INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-8) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
2022-04-19 19:21:10,559 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
2022-04-19 19:21:10,603 INFO  [io.quarkus] (main) Quarkus 2.3.1.Final on JVM started in 4.033s. Listening on: http://localhost:8081
2022-04-19 19:21:10,603 INFO  [io.quarkus] (main) Profile test activated.
2022-04-19 19:21:10,604 INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
2022-04-19 19:21:10,884 INFO  [com.dat.wor.E02_QuarkusCql] (main) Creating the schema...
2022-04-19 19:21:10,929 INFO  [com.dat.wor.E02_QuarkusCql] (main) + [OK]
2022-04-19 19:21:10,929 INFO  [com.dat.wor.E02_QuarkusCql] (main) Inserting Data
2022-04-19 19:21:11,206 INFO  [com.dat.oss.dri.api.cor.uui.Uuids] (main) PID obtained through native call to getpid(): 4465
2022-04-19 19:21:11,238 INFO  [com.dat.wor.E02_QuarkusCql] (main) + [OK]
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 5.023 s - in com.datastax.workshop.E02_QuarkusCql
2022-04-19 19:21:11,258 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Closing Quarkus Cassandra session.
2022-04-19 19:21:11,276 INFO  [io.quarkus] (main) Quarkus stopped in 0.024s
```

## 6.3 - Object Mapping

#### 📘 Ce qu'il faut comprendre:

- Nous construisons un objet annoté avec `@RegisterForReflection` pour permettre la réflexion et les mappers.

```java
@RegisterForReflection
public class Todo {
   private String id;
   private String title;
   private boolean completed;
   // Getter and setters
}
```

- Nous définissions une classe de service `TodoServicesCassandraOM` avec l'annotation `@ApplicationScoped` pour l'introduire dans le contexte de l'application.

- Dans le constructeur nous utilisons le `Mapper` pour instancier un `DAO` créé directement avec le driver.

> ```java
> todoDao = TodoItemMapper
>   .builder(cqlSession)
>   .withDefaultKeyspace(cqlSession.getKeyspace().get())
>   .build()
>   .todoItemDao();
> ```

#### `✅.143` - Utilisation de l'`object mapping` avec `Quarkus`

```bash
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn test -Dtest=com.datastax.workshop.E03_QuarkusObjectMapping
```

#### 🖥️ Logs

```bash
2022-04-19 19:27:49,029 INFO  [io.qua.arc.pro.BeanProcessor] (build-5) Found unrecommended usage of private members (use package-private instead) in application beans:
	- @Inject field com.datastaxdev.todo.TodoRestController#cqlSession,
	- @Inject field com.datastaxdev.todo.TodoRestController#uriInfo
2022-04-19 19:27:49,049 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-4) Micrometer metrics were enabled by configuration, but MicrometerMetricsFactory was not found.
2022-04-19 19:27:49,049 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-4) Make sure to include a dependency to the java-driver-metrics-micrometer module.
2022-04-19 19:27:49,343 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.13.0
2022-04-19 19:27:49,707 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
2022-04-19 19:27:50,596 INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
2022-04-19 19:27:51,258 INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-8) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
2022-04-19 19:27:51,657 INFO  [io.quarkus] (main) Quarkus 2.3.1.Final on JVM started in 3.676s. Listening on: http://localhost:8081
2022-04-19 19:27:51,658 INFO  [io.quarkus] (main) Profile test activated.
2022-04-19 19:27:51,658 INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
2022-04-19 19:27:51,972 INFO  [com.dat.wor.E02_QuarkusCql] (main) Inserting Data
2022-04-19 19:27:52,098 INFO  [com.dat.oss.dri.api.cor.uui.Uuids] (main) PID obtained through native call to getpid(): 4585
2022-04-19 19:27:52,133 INFO  [com.dat.wor.E02_QuarkusCql] (main) + [OK]
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 4.458 s - in com.datastax.workshop.E03_QuarkusObjectMapping
2022-04-19 19:27:52,154 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Closing Quarkus Cassandra session.
2022-04-19 19:27:52,169 INFO  [io.quarkus] (main) Quarkus stopped in 0.021s
```

## 6.4 - Application Quarkus

#### `✅.144` - Démarrer l'application `Quarkus`

- Utiliser le plugin pour démarrer l'application en mode `dev`.

```bash
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn quarkus:dev -DskipTests
```

#### 🖥️ Logs

```bash
2021-12-02 17:53:52,114 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-16) Micrometer metrics were enabled by configuration, but MicrometerMetricsFactory was not found.
2021-12-02 17:53:52,116 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-16) Make sure to include a dependency to the java-driver-metrics-micrometer module.
__  ____  __  _____   ___  __ ____  ______
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
2021-12-02 17:53:52,758 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (Quarkus Main Thread) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.13.0
2021-12-02 17:53:53,067 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (Quarkus Main Thread) Eagerly initializing Quarkus Cassandra client.
2021-12-02 17:53:53,919 INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
2021-12-02 17:53:55,381 INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-8) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
**** Table created true****
2021-12-02 17:53:56,344 INFO  [io.quarkus] (Quarkus Main Thread) javazone-3-quarkus 0.0.1-SNAPSHOT on JVM (powered by Quarkus 2.3.1.Final) started in 5.326s. Listening on: http://localhost:8080
2021-12-02 17:53:56,346 INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
2021-12-02 17:53:56,346 INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]

Tests paused
Press [r] to resume testing, [o] Toggle test output, [h] for more options
```

- L'application démarre et devrait apparaître le tableau de bord de dev.

```bash
gp preview "$(gp url 8081)/q/dev"
```

_Dashboard_
![](img/quarkus-dashboard.png?raw=true)

- Plusieurs plugins sont disponibles directement et notamment `swagger-ui` pour tester l'Api dans un navigateur.

```bash
gp preview "$(gp url 8081)/q/swagger-ui"
```

![](img/quarkus-swagger.png?raw=true)

#### `✅.145` - Test d'intégration avec `Quarkus`

Arrêter l'application en utilisant la touche `q`. Nous pouvons terminer par un test d'intégration

```bash
cd /workspace/conference-2022-devoxx/labs/lab6_quarkus
mvn test -Dtest=com.datastax.workshop.E04_QuarkusController
```

#### 🖥️ Logs

```bash
[INFO] Running com.datastax.workshop.E04_QuarkusController
2022-04-19 21:06:43,421 INFO  [io.qua.arc.pro.BeanProcessor] (build-4) Found unrecommended usage of private members (use package-private instead) in application beans:
	- @Inject field com.datastaxdev.todo.TodoRestController#cqlSession,
	- @Inject field com.datastaxdev.todo.TodoRestController#uriInfo
2022-04-19 21:06:43,444 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-25) Micrometer metrics were enabled by configuration, but MicrometerMetricsFactory was not found.
2022-04-19 21:06:43,444 WARN  [com.dat.oss.qua.dep.int.CassandraClientProcessor] (build-25) Make sure to include a dependency to the java-driver-metrics-micrometer module.
2022-04-19 21:06:43,789 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.13.0
2022-04-19 21:06:45,588 INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
2022-04-19 21:06:46,307 INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-8) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
2022-04-19 21:06:46,696 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
2022-04-19 21:06:46,747 INFO  [io.quarkus] (main) Quarkus 2.3.1.Final on JVM started in 4.790s. Listening on: http://localhost:8081
2022-04-19 21:06:46,748 INFO  [io.quarkus] (main) Profile test activated.
2022-04-19 21:06:46,748 INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 6.577 s - in com.datastax.workshop.E04_QuarkusController
2022-04-19 21:06:48,222 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Closing Quarkus Cassandra session.
2022-04-19 21:06:48,236 INFO  [io.quarkus] (main) Quarkus stopped in 0.020s
```

<p/><br/>

> [🏠 Retour à la table des matières](#-table-des-matières)

# LAB 7 - Micronaut Cassandra

## 7.1 - Introduction à Micronaut

#### 📘 Ce qu'il faut retenir:

- [Micronaut](https://micronaut.io/) est un framework de la JVM pour construire des microservices. Comme Quarkus il vise à construire des applications avec une empreinte mémoire faible et des démarrages ultra rapides. L'idée est de permettre le _serverless_ ainsi que des déploiements dans Kubernetes et le cloud.

- L'approche est différente. Il privilégie l'Aspect Oriented Programming dès la compilation au travers d'`Annotation Processor` (oui comme les mappers). Ainsi de nombreux éléments sont construits à la compilation.

- Pour démarrer avec Micronaut il est utile d'installer [la ligne de commande](https://docs.micronaut.io/latest/guide/index.html#buildCLI) avec `sdkman`.

## 7.2 - Connexion et configuration

#### `✅.146`- Création du keyspace `devoxx_micronaut`

_Dans Docker:_

```sql
CREATE KEYSPACE IF NOT EXISTS devoxx_micronaut
WITH REPLICATION = {
  'class' : 'NetworkTopologyStrategy',
  'dc1' : 3
}  AND DURABLE_WRITES = true;
```

Avec Astra, la manipulation des keyspaces est désactivée, c'est lui qui fixe les facteurs de réplications pour vous (Saas). La procédure est décrite en détail dans [Awesome Astra](https://awesome-astra.github.io/docs/pages/astra/faq/#how-do-i-create-a-namespace-or-a-keyspace) mais voici quelques captures:

_Repérer le bouton `ADD KEYSPACE`_
![](https://awesome-astra.github.io/docs/img/faq/create-keyspace-button.png)

_Créer le keyspace `devoxx_micronaut` et valider avec `SAVE`_
![](https://awesome-astra.github.io/docs/img/faq/create-keyspace.png)

#### `✅.147`- Configuration de l'application `Micronaut`

- Placer vous dans le répertoire `lab7_micronaut` et compiler le projet

```bash
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn clean compile
```

- Localiser le fichier de configuration `application.yml` dans le répertoire `src/main/resources`. C'est le fichier de configuration principal de Micronaut.

```bash
gp open /workspace/conference-2022-devoxx/labs/lab7_micronaut/src/main/resources/application.yml
```

- Suivant la cible (Cassandra dans Docker ou Cassandra dans Astra) la configuration de `micronaut` changera légèrement c'est pourquoi nous avons proposé 2 exemples `application-astra.yml` et `application-local.yml`

- Copier le fichier qui vous correspond vers `application.yml`

```bash
cp /workspace/conference-2022-devoxx/labs/lab7_micronaut/src/main/resources/application-astra.yml /workspace/conference-2022-devoxx/labs/lab7_micronaut/src/main/resources/application.yml
```

ou

```bash
cp /workspace/conference-2022-devoxx/labs/lab7_micronaut/src/main/resources/application-local.yml /workspace/conference-2022-devoxx/labs/lab7_micronaut/src/main/resources/application.yml
```

- Dans le cas de Astra changer la clef `cassandra.default.advanced.auth-provider.password` pour correspondre à votre base. On remarquera que Micronaut on fait le choix d'utiliser les mêmes clefs que le fichier de configuration du drivers et de ne pas réinventer la roue (merci à eux).

#### `✅.148` - Validation de la configuration

> 🚨 The maven test consider the bean NULL. The command below is failin

```
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn test -Dtest=com.datastaxdev.E01_MicronautInit
```

#### 🖥️ Logs

![](img/micronaut_test_01.png?raw=true)

#### `✅.149` - Utilisation de `CqlSession` avec `Micronaut`

> 🚨 The maven test consider the bean NULL. The command below is failin

```
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn test -Dtest=com.datastaxdev.E02_MicronautCql
```

#### 🖥️ Logs

![](img/micronaut_test_02.png?raw=true)

## 7.3 - Object Mapping

#### `✅.150` - Utilisation de l'`object mapping` avec `Micronaut`

> 🚨 The maven test consider the bean NULL. The command below is failin

```bash
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn test -Dtest=com.datastaxdev.E03_MicronautObjectMapping
```

#### 🖥️ Logs

![](img/micronaut_test_02.png?raw=true)

## 7.4 - Application Micronaut

#### `✅.151`- Démarrer l'application `micronaut`

```bash
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn clean compile exec:java
```

#### 🖥️ Logs

```
[INFO] --- exec-maven-plugin:3.0.0:java (default-cli) @ lab7-micronaut ---
 __  __ _                                  _
|  \/  (_) ___ _ __ ___  _ __   __ _ _   _| |_
| |\/| | |/ __| '__/ _ \| '_ \ / _` | | | | __|
| |  | | | (__| | | (_) | | | | (_| | |_| | |_
|_|  |_|_|\___|_|  \___/|_| |_|\__,_|\__,_|\__|
  Micronaut (v3.2.6)

22:28:49.222 [com.datastaxdev.TodoApplication.main()] INFO  c.datastaxdev.TodoApplicationStartup - Startup Initialization
22:28:50.662 [com.datastaxdev.TodoApplication.main()] INFO  c.datastaxdev.TodoApplicationStartup - + Table TodoItems created if needed.
22:28:50.662 [com.datastaxdev.TodoApplication.main()] INFO  c.datastaxdev.TodoApplicationStartup - [OK]
```

- Open the application API on port `8082`

```bash
gp preview "$(gp url 8082)/api/v1/clun/todos/"
```

#### `✅.151` - Test d'intégration avec `Micronaut`

Arrêter l'application en utilisant la touche `CTRL+C`. Nous pouvons terminer par un test d'intégration

```bash
cd /workspace/conference-2022-devoxx/labs/lab7_micronaut
mvn test -Dtest=com.datastaxdev.E04_MicronautController
```

#### 🖥️ Logs

![](img/micronaut_test_04.png?raw=true)

---

Vous êtes à la fin de la session. Félicitations !!

![](img/end.gif?raw=true)

#### `✅.152` - Restons connectés

Si la session vous a plu.

- Rejoignez mon réseau sur [linkedin](https://www.linkedin.com/in/clunven/)
- Twittez à propos de la session avec `@clunven` et `#DevoxxFR`
- Notez la session sur l'application `Devoxx`
