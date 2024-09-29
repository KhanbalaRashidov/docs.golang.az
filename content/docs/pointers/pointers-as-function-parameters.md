---
title: "Pointers as function parameters"
description: ""
summary: ""
date: 2023-11-20T14:41:29+01:00
lastmod: 2023-11-20T14:41:29+01:00
draft: false
weight: 640
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


Go dilində varsayılan olaraq bütün parametrlər funksiyaya dəyər üzrə ötürülür. Məsələn:

```go
package main

import "fmt"

func changeValue(x int) {
    x = x * x
}

func main() {
    d := 5
    fmt.Println("d əvvəl:", d)     // 5
    changeValue(d)                 // dəyəri dəyişməyə çalışırıq
    fmt.Println("d sonra:", d)     // 5 - dəyər dəyişməyib
}
```

Bu nümunədə `changeValue` funksiyası parametri kvadratına yüksəldir, lakin bu, `d` dəyişəninə təsir etmir. Çünki funksiya, `d` dəyişəninin surətini alır və onunla işləyir, orijinal dəyişən dəyişməz qalır.

### Pointerlərdən istifadə edərək dəyişəni dəyişmək

Əgər biz ötürülən dəyişənin dəyərini dəyişmək istəyiriksə, pointerlərdən istifadə edə bilərik:

```go
package main

import "fmt"

func changeValue(x *int) {
    *x = (*x) * (*x)
}

func main() {
    d := 5
    fmt.Println("d əvvəl:", d)     // 5
    changeValue(&d)                // pointeri ilə ötürürük
    fmt.Println("d sonra:", d)     // 25 - dəyər dəyişdi!
}
```

Burada `changeValue` funksiyası `int` tipli obyektin pointeri qəbul edir. Funksiyanı çağırarkən, `d` dəyişəninin ünvanını ötürürük (`changeValue(&d)`), bu da funksiyaya həmin dəyişəni dəyişməyə imkan verir. Nəticədə `d`-nin dəyəri dəyişir.

### Pointeri funksiyadan nəticə kimi qaytarmaq

Funksiya pointer qaytara bilər:

```go
package main

import "fmt"

func createPointer(x int) *int {
    p := new(int)
    *p = x
    return p
}

func main() {
    p1 := createPointer(7)
    fmt.Println("p1:", *p1)     // p1: 7
    p2 := createPointer(10)
    fmt.Println("p2:", *p2)     // p2: 10
    p3 := createPointer(28)
    fmt.Println("p3:", *p3)     // p3: 28
}
```

Bu nümunədə, `createPointer` funksiyası `int` tipli obyektin pointeri qaytarır. Funksiya yeni bir `int` obyekti yaradır, ona dəyər mənimsədir və həmin obyektin pointeri qaytarır.
