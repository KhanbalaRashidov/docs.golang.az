---
title: "Web-api"
url: "/blog/web-api/"
description: "In this guide, we will create a simple web API using Go, which will include basic operations such as creating, reading, updating, and deleting (CRUD) data. We will be using the net/http package for the API and demonstrate how to set up routing and handlers. Additionally, the repository will be accompanied by a README.md file explaining the project setup and structure."
summary: "In this guide, we will create a simple web API using Go, which will include basic operations such as creating, reading, updating, and deleting (CRUD) data. We will be using the net/http package for the API and demonstrate how to set up routing and handlers. Additionally, the repository will be accompanied by a README.md file explaining the project setup and structure."
date: 2024-09-22T20:20:15+02:00
lastmod: 2023-09-22T20:20:15+02:00
draft: false
weight: 50
categories: []
tags: []
contributors: ["Khanbala Rashidov"]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

In this guide, we will create a simple web API using Go, which will include basic operations such as creating, reading, updating, and deleting (CRUD) data. We will be using the `net/http` package for the API and demonstrate how to set up routing and handlers. Additionally, the repository will be accompanied by a `README.md` file explaining the project setup and structure.



We will create a RESTful API that can manage a list of users. The API will expose the following endpoints:

- **GET** `/users`: Fetch all users.
- **GET** `/users/{id}`: Fetch a specific user by ID.
- **POST** `/users`: Create a new user.
- **PUT** `/users/{id}`: Update an existing user by ID.
- **DELETE** `/users/{id}`: Delete a user by ID.



 Step 1: Initialize a Go Module

Start by initializing a Go module for your project:

```bash
mkdir simple-web-api
cd simple-web-api
go mod init github.com/yourusername/simple-web-api
```

Step 2: Set Up the Main File

Create a file named `main.go` where we will define our API routes and handlers:

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
	"net/http"
	"strconv"
	"github.com/gorilla/mux"
)

// User struct representing a user object
type User struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

// In-memory database to store users
var users []User
var nextID = 1

// Get all users
func getUsers(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(users)
}

// Get user by ID
func getUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("Invalid ID"))
		return
	}
	for _, user := range users {
		if user.ID == id {
			w.Header().Set("Content-Type", "application/json")
			json.NewEncoder(w).Encode(user)
			return
		}
	}
	w.WriteHeader(http.StatusNotFound)
	w.Write([]byte("User not found"))
}

// Create a new user
func createUser(w http.ResponseWriter, r *http.Request) {
	var user User
	err := json.NewDecoder(r.Body).Decode(&user)
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("Invalid request body"))
		return
	}
	user.ID = nextID
	nextID++
	users = append(users, user)
	w.Header().Set("Content-Type", "application/json")
	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode(user)
}

// Update user by ID
func updateUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("Invalid ID"))
		return
	}
	for i, user := range users {
		if user.ID == id {
			err := json.NewDecoder(r.Body).Decode(&users[i])
			if err != nil {
				w.WriteHeader(http.StatusBadRequest)
				w.Write([]byte("Invalid request body"))
				return
			}
			users[i].ID = id // Ensure the ID is not modified
			w.Header().Set("Content-Type", "application/json")
			json.NewEncoder(w).Encode(users[i])
			return
		}
	}
	w.WriteHeader(http.StatusNotFound)
	w.Write([]byte("User not found"))
}

// Delete user by ID
func deleteUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("Invalid ID"))
		return
	}
	for i, user := range users {
		if user.ID == id {
			users = append(users[:i], users[i+1:]...)
			w.WriteHeader(http.StatusNoContent)
			return
		}
	}
	w.WriteHeader(http.StatusNotFound)
	w.Write([]byte("User not found"))
}

func main() {
	// Initialize router
	r := mux.NewRouter()

	// API routes
	r.HandleFunc("/users", getUsers).Methods("GET")
	r.HandleFunc("/users/{id}", getUser).Methods("GET")
	r.HandleFunc("/users", createUser).Methods("POST")
	r.HandleFunc("/users/{id}", updateUser).Methods("PUT")
	r.HandleFunc("/users/{id}", deleteUser).Methods("DELETE")

	// Start server
	fmt.Println("Server is running on port 8080")
	log.Fatal(http.ListenAndServe(":8080", r))
}
```

Step 3: Install Dependencies

To manage routing in our API, we will use the `gorilla/mux` package. Install it by running:

```bash
go get -u github.com/gorilla/mux
```

Step 4: Run the Application

After writing the code, you can run the API server:

```bash
go run main.go
```

The API will start running on `http://localhost:8080`.

