---
title: "Anonymous struct"
description: ""
summary: ""
date: 2023-12-12T08:49:59+01:00
lastmod: 2023-12-12T08:49:59+01:00
draft: false
weight: 730
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---



Go dilində **struct** əlaqəli məlumatları qruplaşdırmaq üçün istifadə olunur. Adətən, struct-lar müəyyən bir adla elan olunur və bu, kod daxilində onlara müraciət etməyə və təkrar istifadə etməyə imkan verir. Lakin Go həmçinin **anonim struct**-ları da dəstəkləyir, yəni müəyyən olunmuş adı olmayan struct-lar.

Anonim struct-lar, yalnız müəyyən bir kontekstdə istifadə edilən, sadə və müvəqqəti strukturlar üçün faydalıdır. Onlar əsasən qısamüddətli məlumat strukturları üçün istifadə olunur.

### Anonim Struct Nümunəsi:

```go
package main

import "fmt"

func main() {
    person := struct {
        Name string
        Age  int
    }{
        Name: "Alice",
        Age:  30,
    }

    fmt.Println("Name:", person.Name)
    fmt.Println("Age:", person.Age)
}
```

### Output:
```
Name: Alice
Age: 30
```

Bu nümunədə, anonim struct "Name" və "Age" məlumatlarını saxlamaq üçün yaradılır və `person` dəyişəninə təyin edilir. Struct-ın adı yoxdur və yalnız bu kod blokunda istifadə olunur.
