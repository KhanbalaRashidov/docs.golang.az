---
title: "Goroutines"
description: ""
summary: ""
date: 2023-09-22T12:17:19+02:00
lastmod: 2023-09-22T12:17:19+02:00
draft: false
weight: 940
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


Go dilində goroutine-lər, eyni anda çalışan əməliyyatlardır. Goroutine-lər go açar sözü istifadə edilərək yaradılır və fərqli əməliyyatları eyni vaxtda həyata keçirmək üçün istifadə olunur.

```go
package main

import (
	"fmt"
	"time"
)

func sayHello() {
	fmt.Println("Hello")
}

func main() {
	go sayHello() // goroutine
	time.Sleep(time.Second)
	fmt.Println("World")
}
```

Bu nümunədə, sayHello adlı bir funksiya təyin edilir və "Hello" mesajını ekrana yazdırır.

main funksiyasında, sayHello funksiyası bir goroutine olaraq çağırılır. Bu səbəbdən, sayHello funksiyasının icrası digər əməliyyatlardan müstəqil olaraq baş verir. time.Sleep funksiyası bir saniyəlik gözləmə müddəti əlavə edir. Nəticədə, "World" mesajı ekrana yazdırılır.

Output:

```go
Hello
World
```

Bu nümunədə, goroutine istifadə edərək sayHello funksiyası eyni anda çalışdırıldı. sayHello funksiyası goroutine olaraq çağırıldığı üçün digər əməliyyatlardan müstəqil işləd və nəticədə ekrana "Hello" mesajı yazdırıldıktan sonra "World" mesajı yazdırıldı.


