Sure, let's start with a brief explanation of both:

1. **Chi:** Chi is a lightweight, idiomatic and composable router for building Go HTTP services. It's especially good at helping you declare net/http Handlers while providing much-needed additional functionality for net/http-based web services.

2. **Gorm:** Gorm is a fantastic ORM (Object-Relational Mapping) library for Golang. It provides a simple and flexible way to interact with your database. It supports multiple databases like MySQL, PostgreSQL, SQLite, and more.

Here are some examples on how to use them:

**Chi Example:**

```go
package main

import (
	"net/http"
	"github.com/go-chi/chi"
)

func main() {
	r := chi.NewRouter()

	r.Get("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("welcome"))
	})

	http.ListenAndServe(":3000", r)
}
```
In this example, we've created a new Chi router and registered a GET endpoint at the path "/". The application will start a web server listening on port 3000.

**Gorm Example:**

First, you need to install Gorm by using the following command:

```bash
go get -u gorm.io/gorm
```

Then, you can use it in your Go application:

```go
package main

import (
  "gorm.io/driver/sqlite"
  "gorm.io/gorm"
)

type Product struct {
  gorm.Model
  Code  string
  Price uint
}

func main() {
  db, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }

  // Migrate the schema
  db.AutoMigrate(&Product{})

  // Create
  db.Create(&Product{Code: "D42", Price: 100})

  // Read
  var product Product
  db.First(&product, 1) // find product with integer primary key
  db.First(&product, "code = ?", "D42") // find product with code D42

  // Update - update product's price to 200
  db.Model(&product).Update("Price", 200)

  // Delete - delete product
  db.Delete(&product)
}
```
In this example, we've defined a `Product` struct and performed basic Create, Read, Update, Delete (CRUD) operations using Gorm. The `db.AutoMigrate(&Product{})` line is used to create the table in the database.