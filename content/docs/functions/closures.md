---
title: "Closures Function"
description: ""
summary: ""
date: 2023-12-12T08:31:50+01:00
lastmod: 2023-12-12T08:31:50+01:00
draft: false
weight: 425
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


Closures, Go proqramlaşdırma dilində, bir funksiyanın başqa bir funksiyanın daxilində yaradılması ilə əmələ gələn bir strukturdur. Bu struktur, bir funksiyanın daxilində olan başqa bir funksiyaya istinad edərək, funksiyanın işlədiyi mühitin xaricindəki dəyişənlərə giriş imkanı verir.

```go
func outer() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

increment := outer()
fmt.Println(increment())
fmt.Println(increment())
fmt.Println(increment())
```

Bu örnekte, outer adlı bir fonksiyon tanımlanır. Fonksiyon, bir iç fonksiyon döndürür ve iç fonksiyon, count adlı bir değişkene erişim sağlar. increment adlı bir değişkene outer() fonksiyonu atanır ve bu değişken ile iç fonksiyon çalıştırılır. count değişkeni, increment() çağrıldıkça artar ve her seferinde artışı ekrana yazdırılır.

```go
func adder(a int) func(int) int {
    return func(b int) int {
        return a + b
    }
}

addFive := adder(5)
fmt.Println(addFive(2))
fmt.Println(addFive(3))
```

Bu nümunədə, adder adlı bir funksiya təyin edilir. Funksiya, a adlı bir int tipində parametr qəbul edir və bir daxili funksiya qaytarır. Daxili funksiya, b adlı bir int tipində parametr qəbul edir və a ilə b parametrlərinin cəmini geri qaytarır. addFive adlı bir dəyişənə adder(5) funksiyası təyin edilir və bu dəyişən ilə daxili funksiya işlədilir. addFive(2) və addFive(3) çağırıldıqca nəticələr ekrana yazdırılır.
