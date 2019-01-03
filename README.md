# Google Cloud App Engine connect Cloud SQL

Welcome to [Google Cloud App Engine connect Cloud SQL](https://cloud.google.com/sql/docs/mysql/connect-app-engine?hl=vi) sample code repository. This repository
contain sample code for how to deploy a Spring jpa project connect cloud SQL to Google Cloud by App Engine Service.

# Setup

### Step 1: Create instance cloud SQL
Go to [Cloud SQL](https://console.cloud.google.com/sql/instances) to create instance

### Step 2: Edit pom.xml
- Connect mysql:
    ```sh
    <!--mysql connect-->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<scope>runtime</scope>
	</dependency>
    <!--./-->
    <!--Add dependencies socket factory-->
    <dependency>
        <groupId>com.google.cloud.sql</groupId>
        <artifactId>mysql-socket-factory</artifactId>
        <version>1.0.11</version>
    </dependency>
    <!--./-->
    ```
- Connect postgresql:

    ```sh
     <!--dependencies connect postgresql-->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.5</version>
    </dependency>
    <dependency>
        <groupId>com.google.cloud.sql</groupId>
        <artifactId>postgres-socket-factory</artifactId>
        <version>1.0.11</version>
    </dependency>
    <!--./-->
    ```

### Step 3: Edit appplication.properties

- <database_name>: Name of database you use
- <instance_connection_name>: You can see and copy it at overview tab of sql instance
- <_username> and <_password>: username and password to access to database

- Connection mysql:
    + Add driver class name
        ```sh
        spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
        ```
    + Edit datasource url
        ```sh
        spring.datasource.url=jdbc:mysql://google/<database_name>?\
                    cloudSqlInstance=<instance_connection_name>&\
                    socketFactory=com.google.cloud.sql.mysql.SocketFactory&\
                    useSSL=false&user=<_username>&password=<_password>
        ```
    + The SQL dialect makes Hibernate generate better SQL for the chosen database
        ```sh
        spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
        ```

- Connect postgresql:
    + Edit datasource url
        ```sh
        spring.datasource.url=jdbc:postgresql://google/<database_name>?\
                    cloudSqlInstance=<instance_connection_name>&\
                    socketFactory=com.google.cloud.sql.postgres.SocketFactory&\
                    useSSL=false&user=<_username>&password=<_password>
        ```
    + Remove "wall of text"
        ```sh
        spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
        ```

# Run locally

* Before run locally you should run command:
    ```sh
    $ mvn clean
    ```
* You can run locally by run command:
    ```sh    
    $ mvn -DskipTests spring-boot:run
    ```
* You can see result at Preview on port 8080 by click eye icon at top-right cloud shell

# Deploy to Google Cloud App Engine 

* Add and config app.yaml in main/appengine/app.yaml, you can read more at [App Engine Flexible](https://cloud.google.com/appengine/docs/flexible/java/configuring-your-app-with-app-yaml)
* Add plugin appengine in pom.xml
    ```sh
    <!--Add plugin appengine to deploy gooogle cloud-->
    <plugin>
	    <groupId>com.google.cloud.tools</groupId>
		<artifactId>appengine-maven-plugin</artifactId>
		<version>1.3.2</version>
	</plugin>
    <!--./-->
    ```
* And now to deploy to App Engine Service by run command:
    ```sh
    $ mvn -DskipTests appengine:deploy
    ```
* To show link result when deploy successfully:
    ```sh
    $ glcoud browse app
    ```
