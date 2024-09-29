---
title: "Web-api"
url: "/blog/web-api-az/"
description: ""
summary: "Bu təlimatda Go dilindən istifadə edərək sadə bir veb API yaradacağıq və bu API-də məlumatları yaratmaq, oxumaq, yeniləmək və silmək (CRUD) əməliyyatları həyata keçirəcəyik. Biz `net/http` paketindən istifadə edəcəyik və marşrutları (routes) və işləyiciləri (handlers) necə qurmağı göstərəcəyik. Layihə ilə birlikdə `README.md` faylı da olacaq, burada layihənin qurulması və strukturu izah ediləcək."
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

Bu təlimatda Go dilindən istifadə edərək sadə bir veb API yaradacağıq və bu API-də məlumatları yaratmaq, oxumaq, yeniləmək və silmək (CRUD) əməliyyatları həyata keçirəcəyik. Biz `net/http` paketindən istifadə edəcəyik və marşrutları (routes) və işləyiciləri (handlers) necə qurmağı göstərəcəyik. Layihə ilə birlikdə `README.md` faylı da olacaq, burada layihənin qurulması və strukturu izah ediləcək.

### API Əməliyyatları:

API aşağıdakı marşrutları təmin edəcək:

- **GET** `/users`: Bütün istifadəçiləri gətir.
- **GET** `/users/{id}`: Müəyyən bir istifadəçini ID-yə görə gətir.
- **POST** `/users`: Yeni istifadəçi yarat.
- **PUT** `/users/{id}`: Mövcud istifadəçini ID-yə görə yenilə.
- **DELETE** `/users/{id}`: Müəyyən bir istifadəçini ID-yə görə sil.

### Addım 1: Go Modulu Yaradın

Layihəniz üçün Go modulunu başladın:

```bash
mkdir simple-web-api
cd simple-web-api
go mod init github.com/yourusername/simple-web-api
```

### Addım 2: Əsas Faylı Qurun

API marşrutlarını və işləyicilərini təyin edəcəyimiz `main.go` adlı fayl yaradın:

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

// İstifadəçi strukturu
type User struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

// İstifadəçilərin saxlanıldığı yaddaş
var users []User
var nextID = 1

// Bütün istifadəçiləri gətirən funksiya
func getUsers(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(users)
}

// ID ilə istifadəçi gətirən funksiya
func getUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("ID səhvdir"))
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
	w.Write([]byte("İstifadəçi tapılmadı"))
}

// Yeni istifadəçi yaradan funksiya
func createUser(w http.ResponseWriter, r *http.Request) {
	var user User
	err := json.NewDecoder(r.Body).Decode(&user)
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("Səhv sorğu formatı"))
		return
	}
	user.ID = nextID
	nextID++
	users = append(users, user)
	w.Header().Set("Content-Type", "application/json")
	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode(user)
}

// ID ilə istifadəçini yeniləyən funksiya
func updateUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("ID səhvdir"))
		return
	}
	for i, user := range users {
		if user.ID == id {
			err := json.NewDecoder(r.Body).Decode(&users[i])
			if err != nil {
				w.WriteHeader(http.StatusBadRequest)
				w.Write([]byte("Səhv sorğu formatı"))
				return
			}
			users[i].ID = id
			w.Header().Set("Content-Type", "application/json")
			json.NewEncoder(w).Encode(users[i])
			return
		}
	}
	w.WriteHeader(http.StatusNotFound)
	w.Write([]byte("İstifadəçi tapılmadı"))
}

// ID ilə istifadəçini silən funksiya
func deleteUser(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	id, err := strconv.Atoi(params["id"])
	if err != nil {
		w.WriteHeader(http.StatusBadRequest)
		w.Write([]byte("ID səhvdir"))
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
	w.Write([]byte("İstifadəçi tapılmadı"))
}

func main() {
	// Routeri başlat
	r := mux.NewRouter()

	// API marşrutları
	r.HandleFunc("/users", getUsers).Methods("GET")
	r.HandleFunc("/users/{id}", getUser).Methods("GET")
	r.HandleFunc("/users", createUser).Methods("POST")
	r.HandleFunc("/users/{id}", updateUser).Methods("PUT")
	r.HandleFunc("/users/{id}", deleteUser).Methods("DELETE")

	// Serveri başlat
	fmt.Println("Server 8080 portunda işləyir")
	log.Fatal(http.ListenAndServe(":8080", r))
}
```

### Addım 3: Asılılıqları Yükləyin

Marşrutları idarə etmək üçün `gorilla/mux` paketindən istifadə edəcəyik. Onu aşağıdakı əmr vasitəsilə quraşdırın:

```bash
go get -u github.com/gorilla/mux
```

### Addım 4: Tətbiqi İşə Salın

Kodu yazdıqdan sonra API serverini işə salmaq üçün aşağıdakı əmri istifadə edin:

```bash
go run main.go
```

API `http://localhost:8080` ünvanında işə düşəcək.
