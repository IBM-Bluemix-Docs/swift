---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-21"

keywords: sql swift, database swift, persistence swift, data swift, orm swift, kuery swift, kitura swift

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Using an SQL database for data persistence
{: #sql_data}

Structured Query Language (SQL) is a domain-specific language that is used for managing data in relational databases. Data persistence is recommended for when your server shuts down while you're using it. To add data persistence, you can use an SQL Database directly from Swift. 

One of the most important features from Swift is its type-safety. Using an SQL database with Swift is a logical choice because type-safety is supported by both.

## Using ORM with an SQL Database
{: sql-orm}

With Object-Relational Mapping (ORM), you can map objects to relational databases without having to deal with SQL statements. You can then store and retrieve objects from a relational database without doing much of the parsing and serialization yourself.

## Step 1. Getting started with ORM
{: #start-orm}

Use the [Swift-Kuery-ORM](https://github.com/IBM-Swift/Swift-Kuery-ORM){: external} with an SQL plug-in such as [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: external} or [MySQL](https://github.com/IBM-Swift/SwiftKueryMySQL){: external}.

For this example, the [PostgreSQL](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL){: external} plug-in is used. Follow the instructions to [install the plug-in](https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL#postgresql-client-installation){: external}.

## Step 2. Importing ORM into your application
{: #import-orm}

1. Update the `Package.swift` file by adding the `Swift-Kuery-ORM` and `Swift-Kuery-PostgreSQL` packages:
  ```swift
     dependencies: [
      ...
      /* Add these two lines */
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-ORM.git", from: "0.0.1"),
      .package(url: "https://github.com/IBM-Swift/Swift-Kuery-PostgreSQL.git", from: "1.0.0"),
    ],
    targets: [
      .target(
        name: ...
        /* Add these two modules to your target(s) dependencies */
        dependencies: [..., "SwiftKueryORM", "SwiftKueryPostgreSQL"]),
    ]
  ```
  {: codeblock}

2. To add ORM functionality, add the following `import` lines at the beginning of your `Application.swift`:
  ```swift
  import SwiftKueryORM
  import SwiftKueryPostgreSQL
  ```
  {: codeblock}

## Step 3. Creating your database
{: #create-db-sql}

1. After PostgreSQL is set up on your machine, use a terminal to create the database:
  ```
  brew services start postgresql
  createdb school
  ```
  {: pre}

2. Initialize the database in your `Application.swift`:
  ```swift
  let pool = PostgreSQLConnection.createPool(host: "localhost", port: 5432, options: [.databaseName("school")], poolOptions: ConnectionPoolOptions(initialCapacity: 10, maxCapacity: 50, timeout: 10000))
  Database.default = Database(pool)
  ```
  {: codeblock}

  **Note**: A connection pool is used for concurrent requests.

## Step 4. Defining your model
{: #define-model-sql}

1. Create the model, `Grade`:
  ```swift
  struct Grade: Model {
    var course: String
    var grade: Int
  }
  ```
  {: pre}

2. Then, create your table:
  ```swift
  do {
    try Grade.createTableSync()
  } catch {
    /* Error */
  }
  ```
  {: pre}

## Step 5. Managing your data
{: #manage-data-sql}

### Saving data
{: #save-data-sql}

To save an instance of `Grade`, create an instance, and call `save()`:
```swift
let grade = Grade(course: "Computer Science", grade: 76)

// Save to the database
grade.save { student, error in
  ...
}
```
{: pre}

### Finding data
{: #find-data-sql}

To retrieve all the Grades from the database, you can use the static call, `findAll()` :
```swift
// Get an array of Grades in the database
Grade.findAll { students, error in
  ...
}
```
{: pre}

### Updating data
{: update-data-sql}

A similar approach is used to update a Grade from the database:
```swift
grade.grade = 80

// Update the Grade
Grade.update(id: 1) { student, error in
  ...
}
```
{: pre}

### Deleting data
{: #delete-data-sql}

A similar approach is used to delete a Grade from the database.
```swift
// Delete the Grade
Grade.delete(id: 1) { error in
  ...
}
```
{: pre}

All of these calls take a handler that is called once and runs it when the database call is complete.

## Using Swift-Kuery directly
{: #swift-kuery}

If ORM limits you because you need more control over your database, you can use the SQL abstraction layer, [Swift-Kuery](https://github.com/IBM-Swift/Swift-Kuery){: external}, where you can make an SQL query.
