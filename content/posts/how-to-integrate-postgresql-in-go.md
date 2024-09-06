---
title: "Integrating PostgreSQL with Go"
date: "2024-08-29"
summary: "PostgreSQL Integration in Go"
description: "PostgreSQL Integration in Go"
toc: true
readTime: true
autonumber: true
math: true
tags: ["programming", "go", "tips", "postgresql"]
showTags: false
hideBackToTop: false
---

## Introduction

PostgreSQL, an open-source relational database system, is widely used by developers for its performance and dependability. Integrating it with Go, a statically typed programming language, allows for high-performance applications.

An issue that beginners frequently run into when combining PostgreSQL with Go is effectively managing database connections. It is tempting to initiate numerous database connections at once with Go's concurrency model, but this can soon result in resource exhaustion or performance problems if connection pooling and management aren't done properly.

Beginners may also find it difficult to ensure that their queries are protected from SQL injection attacks or to handle SQL errors gracefully. For anyone working with PostgreSQL in Go, knowing how to set up the connection pool correctly, write secure SQL statements, and handle failures well are essential.

In this article, I'll guide you through establishing a connection to PostgreSQL using Go, making the most of its powerful features. We’ll cover everything from setting up a PostgreSQL instance using Docker to configuring your Go project, connecting Go with PostgreSQL, writing secure queries to prevent SQL injection, and much more. So, without further ado, let's get started!

## Setting Up A Project

1. Setting Up a PostgreSQL via Docker
   
   I assume you're already familiar with Docker and know how to start the Docker engine. If not, you can [download](https://www.postgresql.org/download) PostgreSQL locally and follow the installation instructions provided by the [official documentation](https://www.postgresql.org/docs). For the purposes of this tutorial, I'll be using Docker to set up PostgreSQL.

   First and foremost, you'll want to start Docker and pull the PostgreSQL Docker image. The command will look like this:

   ```shell
    $ docker run --name pg -e POSTGRES_PASSWORD=123 -p 5432:5432 -d postgres
   ```

   Feel free to name your PostgreSQL instance as you like; the container name is set after the `--name` flag. In this example, I've named the instance `pg` because it's short and easy to remember. The next step is to configure the database password by setting the `POSTGRES_PASSWORD` environment variable. Here, I've used `123` for simplicity, but in a real-world scenario, you'll want to choose a more secure password.

   To verify that the Docker container is up and running, use the following command:

   ```shell
    $ docker ps
   ```

2. Creating a Database in PostgreSQL
   
   Once your Docker instance is up and running, it's time to create your database. You can do this by accessing the terminal within your PostgreSQL container. To do so, run the following command:

   ```shell
    $ docker exec -it <your container name> bash
   ```

   Once inside the container, connect to the PostgreSQL server using the `psql` command:

   ```shell
    $ psql -U <your postgres username>
   ```

   After accessing your PostgreSQL terminal, you can create a new database by typing the following command:

   ```sql
    CREATE DATABASE pg_go;
   ```

   In this tutorial, the database is named `pg_go` for demonstration purposes, but feel free to use any name that suits your needs.

   To verify that your new database has been created, list all the databases available within your PostgreSQL container by using the following command:

   ```shell
    $ \l
    $ \q # To quit the PostgreSQL interactive terminal
   ```

   If your newly created database appears in the list, congratulations! You've successfully set up PostgreSQL in a Docker container and created a database within it.

3. Creating a Go Project
   
   As the title suggests, it's time to create your Go project. I won't go into the details, assuming you're already familiar with Go or even have an existing project.

   However, if you don't have a project yet, now is the perfect time to start one!

Alright, we've set up all the necessary requirements in both PostgreSQL and Go. Now it's time to dive deeper into the technical details of how PostgreSQL interacts with Go. Get ready—we're going to do a lot of coding!

## Libraries

When it comes to connecting PostgreSQL with Go, there are several libraries available, each with its own strengths. In this tutorial, we'll focus on `pgx`, a popular and powerful library for interacting with PostgreSQL databases in Go.

`pgx (PostgreSQL Extension for Go)` is known for its performance and feature-rich capabilities. Here are some of the key advantages of using `pgx`:

1. Performance: 
   
   `pgx` is designed with performance in mind. It offers a highly optimized and efficient interface for PostgreSQL, often resulting in faster query execution and reduced overhead compared to other libraries.

2. Native PostgreSQL Support: 
   
   `pgx` provides native support for PostgreSQL features, including advanced data types and custom types. This allows for more seamless and direct interactions with PostgreSQL's capabilities.

3. Connection Pooling: 
   
   Built-in connection pooling is a major advantage of `pgx`. This feature helps manage database connections efficiently, reducing latency and improving the scalability of your application.

You can install `pgx` using the following command:

   ```shell
    $ go get github.com/jackc/pgx/v5
   ```

For many users, this command works perfectly fine. However, if you encounter issues, you might need to include the `stdlib` sub-package:

   ```shell
    $ go get github.com/jackc/pgx/v5/stdlib
   ```

The `stdlib` sub-package provides compatibility with Go's `database/sql` interface. Some projects require this additional package for proper integration, especially if your code relies on the standard SQL interface. While many users won't need this, including it can resolve potential issues.

## Code Setup

Let's dive into the code and explore how to configure PostgreSQL in our Go application.

First, we'll create a `.env` file to securely store our database credentials.

```.env
# Database Management
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=123
DB_NAME=pg_go
```

Ensure that the credentials match your own database credentials.

We'll need to install an additional dependency to load our `.env` file into our Go program. We'll use `godotenv` for this purpose. To install the dependency, run the following command:

   ```shell
    $ go get github.com/joho/godotenv
   ```

Let's create a folder named `database` to store our configuration file, which we'll call `config.go`.

Inside `config.go`, define a struct to represent your database credentials, specifying the appropriate data types for each field.

   ```go
   package database

   type DBConfig struct {
      host     string
      port     string
      username string
      password string
      name     string
   }
   ```

You might be wondering why we use a string for the `port` field when it’s clearly a number. Shouldn't it be an `int`?

While it is true that ports are numerical values, the `port` field is defined as a string because **environment variables**, where these values are often stored, are typically in string format. Keeping `port` as a string simplifies the process of reading configuration data from environment variables, eliminating the need for conversion from a string to an integer.

To manage our database configuration efficiently, we need to create two functions: a constructor for loading the configuration and a method to generate the database [connection URI](https://stackoverflow.com/questions/3582552/what-is-the-format-for-the-postgresql-connection-string-url).

The `NewDBConfig` function is responsible for loading environment variables and creating a new instance of `DBConfig` struct. Here’s how it works:

   ```go
   func NewDBConfig() *DBConfig {
      err := godotenv.Load()
      if err != nil {
         log.Printf("Error when reading .env file: %v\n", err)
      }

      return &DBConfig{
         host:     os.Getenv("DB_HOST"),
         port:     os.Getenv("DB_PORT"),
         username: os.Getenv("DB_USERNAME"),
         password: os.Getenv("DB_PASSWORD"),
         name:     os.Getenv("DB_NAME"),
      }
   }
   ```

- It loads environment variables from a `.env` file, which is a common practice for managing configuration settings outside the codebase.

- It logs an error message if the `.env` file cannot be read, allowing the application to handle configuration issues gracefully.

- It returns a new `DBConfig` instance with values obtained from the environment variables, centralizing configuration management.

The `GetDBURI` method constructs and returns a formatted database [connection URI](https://stackoverflow.com/questions/3582552/what-is-the-format-for-the-postgresql-connection-string-url). Here’s the method:

   ```go
   func (config *DBConfig) GetDBURI() string {
      return fmt.Sprintf("postgres://%s:%s@%s:%s/%s",
      config.username, config.password, config.host, config.port, config.name)
   }
   ```

- It creates a [connection URI](https://stackoverflow.com/questions/3582552/what-is-the-format-for-the-postgresql-connection-string-url) in the format **"postgres://username:password@host:port/dbname"**, which is [required for connecting to the PostgreSQL database](https://github.com/jackc/pgx?tab=readme-ov-file#example-usage).

- This method simplifies the process of constructing the URI, ensuring consistent and error-free string formatting throughout your application.

Alright, we’ve completed setting up our PostgreSQL configuration in Go.

## Code to Connect

Next, let's write the code to establish a connection to the database in Go.

First, create a `main.go` file in the root directory. Inside it, define a main function, and within that function, call the `NewDBConfig` function from your `database` package. Here's how it should look:

   ```go
   dbConfig := database.NewDBConfig()
   ```

Before we dive deeper, you'll need to import the following packages: `database/sql` and `github.com/jackc/pgx/v5/stdlib`. I’m using the `pgx` standard library wrapper `(stdlib)` because I encountered some issues without using `stdlib`.

   ```go
   import (
      "database/sql"
      
      "github.com/ezrantn/pg_go/database"
      _ "github.com/jackc/pgx/v5/stdlib"
   )
   ```
Now, you might notice the underscore `_` before the `pgx` import. Why do we do this? The underscore is used to import the package solely for its side effects, meaning we’re not using the package **directly** in our code. Instead, we rely on it **indirectly**. 

This might sound confusing, but in Go, some packages register themselves with the `database/sql` package when imported. The `pgx` package does this through an init function, so even though we don’t call `pgx` directly, importing it with `_` ensures that it still runs its initialization code and registers the driver with `database/sql`.

Alright, if that sounds good to you, let's continue with our code. Next, we'll connect to our PostgreSQL database using the `sql.Open` method. We'll pass the `pgx` driver as a string along with our database URI.

   ```go
   db, err := sql.Open("pgx", dbConfig.GetDBURI())
   if err != nil {
	  log.Printf("Failed to connect database: %v\n", err)
	  return
   }
   
   defer db.Close()
   
   if err := db.Ping(); err != nil {
      log.Printf("Failed to connect database: %v\n", err)
	  return
   }
   
   fmt.Println("Database is successfully connected")
   ```

A lot is happening here, so let's break it down:

- The defer `db.Close()` statement ensures that the database connection is properly closed when the surrounding function completes, freeing up any resources.

- The code then checks if the database connection is valid by sending a ping using `db.Ping()`.

- If the ping fails, error message is logged, and the function returns early, indicating that the database connection was unsuccessful.

- If all previous steps succeed, a message is printed to the console confirming that the database connection has been successfully established.

Now, when you run this program, cross your fingers, and hopefully, you'll see the message `Database is successfully connected`. If you do, it means our code is working, and PostgreSQL is officially connected in Go.

If you encounter an error, ensure that your PostgreSQL instance is up and running in Docker and that your credentials match those in your `.env` file.

From here you can perform a query to PostgreSQL but in Go, we're going to look that later.

## Connection Pool

Let's discuss connection pooling and its importance for setting up database connections in your application.

A connection pool is a collection of reusable database connections that your application can use to interact with the database. Instead of opening and closing a new connection every time your application needs to interact with the database, it borrows a connection from the pool, uses it, and then returns it to the pool for future use.

Opening a new database connection is resource-intensive and can take time. If your application is handling many requests simultaneously, opening and closing connections for each request could slow things down significantly. A connection pool helps by maintaining a set of open connections that can be reused, which improves the performance and scalability of your application.

Imagine a coffee shop with only one barista. Every time a customer arrives, the barista starts making coffee from scratch, one cup at a time. If 10 customers show up at once, each has to wait for the barista to finish with the previous customer before their coffee is made. This is like opening and closing a database connection for each request—slow and inefficient.

Now, imagine the coffee shop hires more baristas. When customers arrive, they are assigned to the next available barista. The baristas keep working until they finish, and when they're done, they’re ready to help the next customer. This is like a connection pool—multiple connections (baristas) are available to handle requests (customers) simultaneously, speeding up the process.

Here’s how you can set up a connection pool in your application:

1. Setting the maximum number of open connections

   - `DB.SetMaxOpenConns`: The database limit sets a threshold for open connections, allowing new operations to wait for an existing one to finish before creating a new one. By default, all existing connections are used when needed, but setting a limit can cause deadlock in the application.

2. Setting the maximum number of idle connections

   - `DB.SetMaxIdleConns`: The limit on idle connections `sql.DB` maintains can be adjusted to prevent frequent reconnects in programs with significant parallelism. By default, the database keeps two idle connections, but raising the limit can prevent frequent connections when an SQL operation finishes.

3. Setting the maximum amount a time a connection can be idle

   - `DB.SetConnMaxIdleTime`: Sets the maximum idle time a connection can be before being closed, allowing `sql.DB` to close idle connections. By default, idle connections remain in the pool until needed. Using `DB.SetConnMaxIdleTime` can increase idle connections during parallel activity bursts and release them later when the system is quiet.

4. Setting the maximum lifetime of connections

   -  `DB.SetConnMaxLifetime`: The connection's maximum open time before closure is set, allowing for arbitrary reuse. However, in load-balanced database servers, it's crucial to prevent applications from using a connection for too long without reconnecting, ensuring proper connection management.
  
   ```go
   db.SetMaxOpenConns(10) // maximum 10 open connections
   db.SetMaxIdleConns(5) // maximum number of idle connections in the pool
   db.SetConnMaxIdleTime(15 * time.Minute) // maximum idle time a connection can be before being closed
   db.SetConnMaxLifetime(1 * time.Hour) // connection's maximum lifetime before closure
   ```

While there's no exact formula for determining the ideal number of connections and idle time, it depends on your specific application and environment. It's essential to monitor your application's performance and adjust your connection pool settings as needed.

But here are some general guidelines, or "rules of thumb," that you can follow:

1. Number of Connections

   - As a general rule, it's a good idea to start with a small number of connections (e.g., 10-20) and adjust based on your application's performance and load.

   - Consider the number of concurrent requests your application can handle and the average duration of each request.
    
   - If your application is highly concurrent, you may want to start with a higher number of connections.
  
2. Idle Time

   - The idle time should be long enough to allow for efficient connection reuse, but not so long that it wastes resources.
  
   - A good starting point is 5-30 minutes. This allows for a reasonable amount of connection reuse while still allowing for efficient connection management.
  
3. Connection Lifetime

   - The connection lifetime should be long enough to allow for efficient connection reuse, but not so long that it wastes resources or causes issues with connection pooling.
  
   - A good starting point is 1-2 hours. This allows for a reasonable amount of connection reuse while still allowing for efficient connection management.
  
4. Monitoring and Adjustment
   
   - Pay attention to metrics such as request latency, request throughput, and connection pool utilization to determine the optimal settings for your application.
  
5. Database Connection Pooling

   - Consider the specific database you're using and its connection pooling capabilities.
  
   - For example, PostgreSQL has its own connection pooling mechanism, and you may want to use that instead of creating your own connection pool.
  
6. Testing and Iteration

   - Test your application under different loads and scenarios to determine the optimal connection pool settings.
  
   - Iterate on your settings based on the results of your testing and monitoring.

## Database Migration

Database migrations in Go are surprisingly straightforward. In fact, you can simply write SQL queries as you're used to doing. I'll demonstrate this. 

Within the `database` package we created earlier, let's create a new file called `migrate.go`. Inside this file, we'll define a function that handles database migrations. 

Although we're not relying on any external dependencies, we still need to craft a SQL query to create our tables. 

Here's an example of how to achieve this:

```go
func Migrate(db *sql.DB) (sql.Result, error) {
	if db == nil {
		return nil, errors.New("internal server error")
	}

	return db.Exec(`
	CREATE TABLE IF NOT EXISTS products (
			id VARCHAR(36) PRIMARY KEY,
			name VARCHAR(255) NOT NULL,
			price BIGINT NOT NULL,
			is_deleted BOOLEAN NOT NULL DEFAULT FALSE
	);
   `) 
}
```

- The `Migrate` function ensures that the database structure is properly set up by executing a `CREATE TABLE` SQL statement. At its core, this function utilizes `db.Exec`, which is designed to execute SQL statements that don't return rows (like CREATE, INSERT, UPDATE, DELETE).
  
- The `IF NOT EXISTS` clause prevents errors by ensuring that the table is only created if it doesn't already exist in the database. 
  
- When executing the query, `db.Exec` returns two values: `sql.Result`, which provides information about the execution result, such as the number of rows affected; and an `error`, which is returned if the SQL command fails (e.g., due to syntax errors or database connection issues).

The next step is to execute the `Migrate` function from our `main` method, as shown below:

```go
if _, err = database.Migrate(db); err != nil {
	fmt.Printf("Migration failed to complete: %v\n", err)
	os.Exit(1)
}
```

If no errors occur, the migration is successful, and you can now view the products table in your database. 

Congratulations on completing your first Go database migration!

## Executing Queries

When executing an SQL statement that returns data, use one of the `Query` methods provided in the `database/sql` package. These methods return a `Row` or `Rows` object, which can be scanned to extract data using the `Scan` method. For example, you can use these methods to execute `SELECT` statements.

In contrast, when performing database actions that don't return data, such as inserting, deleting, or updating records, use the `Exec` or `ExecContext` methods from the `database/sql` package. These methods execute SQL statements without returning data.

Let's take a closer look at an example of using the `QueryRow` method to retrieve data.

This following function returns a single row:

```go
func SelectProductByID(db *sql.DB, id string) (Product, error) {
	if db == nil {
		return Product{}, ErrDBNil
	}

	query := `SELECT id, name, price FROM products WHERE is_deleted = false AND id = $1;`
	row := db.QueryRow(query, id)

	var product Product
	if err := row.Scan(&product.ID, &product.Name, &product.Price); err != nil {
		return Product{}, err
	}

	return product, nil
}
```

A lot is going on, so let's break it down to make it more manageable:

- The `QueryRow` method is used to execute a SQL query that is expected to return **only a single row**. It retrieves the first row from the result set and discards the rest.

- `$1` is a placeholder for a parameterized query. This means the query will be securely parameterized with the id variable, helping **prevent SQL injection attacks**.

- After `QueryRow` is called, the result is stored in row. To extract the values from the row, the `Scan` method is used. This method scans the values from the result into the specified variables for this example (`&product.ID, &product.Name, &product.Price`).

- The `Scan` method must have arguments that correspond to the number, type, and order of columns returned by the query. In this case, the query selects three columns: id, name, and price. The order of variables passed to `Scan` **must exactly match** the order of columns in the SQL query.

> **NOTE**: Parameter placeholders in prepared statements vary depending on the DBMS and driver you’re using. For example, the `go-sql-driver` for MySQL requires a placeholder like ? instead of $1.

Next, we'll explore how to retrieve all the data using the `Query` method.

Specifically, this method returns a slice of `[]Product` objects, which contains all the data from your database.

```go
func SelectProducts(db *sql.DB) ([]Product, error) {
	if db == nil {
		return nil, ErrDBNil
	}

	query := `SELECT id, name, price FROM products WHERE is_deleted = false;`

	rows, err := db.Query(query)
	if err != nil {
		return nil, err
	}

	products := []Product{}

	for rows.Next() {
		var product Product
		err := rows.Scan(&product.ID, &product.Name, &product.Price)
		if err != nil {
			return nil, err
		}

		products = append(products, product)
	}

	return products, nil
}
```

Upon closer inspection, you'll notice that `Query` method behaves similarly to `QueryRow`, with the key difference being that it returns a **collection of rows** instead of a single row. 

Let's dive deeper into the specifics of each function to better understand their differences:

- This query selects three columns: id, name, and price from the products table, filtering for rows where `is_deleted = false`. It retrieves multiple rows since we are using `db.Query`, which is meant for queries that return **multiple rows**.

- Since `db.Query` is used to fetch multiple rows, `rows.Next` is used to iterate over each row in the result set.

- In each iteration of the loop, `rows.Scan` extracts values from the current row of the result set and assigns them to the fields of a `Product` struct.

- Just like with `QueryRow`, the order of variables passed to `Scan` in each iteration must exactly match the order of columns in the SQL query.

Let's take a coffee break for a moment... ☕

Now, let's dive into executing queries to insert, update, or delete data from our database. 

A great place to start is with an example of inserting data, which is demonstrated below:

```go
func InsertProduct(db *sql.DB, product Product) error {
	if db == nil {
		return ErrDBNil
	}

	query := `INSERT INTO products (id, name, price) VALUES ($1, $2, $3);`

	_, err := db.Exec(query, product.ID, product.Name, product.Price)
	if err != nil {
		return err
	}

	return nil
}
```

- Unlike `Query` or `QueryRow`, which are used for SQL statements that return rows of data, `Exec` is used for executing SQL statements that **do not return rows** (e.g., INSERT, UPDATE, DELETE).

- The query uses placeholders $1, $2, and $3, which correspond to the parameters `product.ID, product.Name`, and `product.Price`. This helps prevent SQL injection by securely parameterizing the query.

- The arguments passed after the query (`product.ID, product.Name, product.Price`) are used to fill in the placeholders $1, $2, and $3, respectively.

- The `Exec` method returns two values, each with a specific purpose. The first value, result, is typically discarded (as seen in this case with the _ placeholder), but it represents the outcome of the SQL execution, including information about the number of rows affected by the operation. The second value, error (or err), is returned if the execution fails for any reason, such as constraint violations or connection issues. It is essential to properly handle this error to ensure robust and reliable database interactions.

>**NOTE**: While we've covered inserting data, I'll skip discussing deletion and update operations for now, as the underlying `Exec` method remains the same. Instead, the differences lie in the query and logic used.

That's a it! We've now explored how to execute queries in Go using the `sql` package.

And that's a wrap! I hope you've learned something new about integrating PostgreSQL with Go. If you have any feedback or suggestions for this article, please don't hesitate to reach out. I appreciate you taking the time to read this. Happy hacking! 👋