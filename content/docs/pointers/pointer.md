---
title: "Pointer"
description: ""
summary: ""
date: 2024-02-13T15:39:07+01:00
lastmod: 2024-02-13T15:39:07+01:00
draft: false
weight: 610
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


Pointerlər, dəyərləri digər obyektlərin (məsələn, dəyişənlərin) ünvanları olan obyektlərdir.

Pointeri adi dəyişən kimi müəyyən edilir, lakin məlumat tipinin qarşısına ulduz işarəsi (*) qoyulur. Məsələn, `int` tipli obyektə pointerin müəyyən edilməsi:

```go
var p *int
```

Bu pointerə `int` tipli dəyişənin ünvanını mənimsətmək olar. Ünvanı əldə etmək üçün `&` əməliyyatı istifadə edilir, onun ardınca isə dəyişənin adı yazılır (`&x`).

```go
package main

import "fmt"

func main() {

    var x int = 4       // dəyişəni müəyyən edirik
    var p *int          // pointeri müəyyən edirik
    p = &x              // pointer dəyişənin ünvanını alır
    fmt.Println(p)      // pointerin dəyəri - dəyişən x-in ünvanı
}
```

Burada pointeri `p`, `x` dəyişəninin ünvanını saxlayır. Əsas məqam budur ki, `x` dəyişəni `int` tipinə malikdir və pointer `p` məhz `int` tipli obyektə işarə edir. Yəni tip uyğunluğu olmalıdır. Əgər dəyişənin ünvanını konsolda çap etməyə çalışsaq, onun onaltılıq dəyər olduğunu görəcəyik:

```
0xc0420120a0
```

Hər bir halda ünvan fərqli ola bilər, amma mənim nümunəmdə `x` dəyişəninin maşın ünvanı `0xc0420120a0` olub. Yəni kompyüterin yaddaşında `x` dəyişəninin yerləşdiyi ünvan `0xc0420120a0`-dir.

Pointerdə saxlanılan ünvan vasitəsilə biz `x` dəyişəninin dəyərini əldə edə bilərik. Bunun üçün * (dereferencing) əməliyyatı tətbiq edilir. Bu əməliyyatın nəticəsi, pointerin işarə etdiyi dəyişənin dəyəridir. Bu əməliyyatı tətbiq edərək `x` dəyişəninin dəyərini əldə edək:

```go
package main

import "fmt"

func main() {

    var x int = 4
    var p *int  = &x                // pointer dəyişənin ünvanını alır
    fmt.Println("Ünvan:", p)        // pointerin dəyəri - x-in ünvanı
    fmt.Println("Dəyər:", *p)       // x-in dəyəri
}
```

Output:

```
Ünvan: 0xc0420c058
Dəyər: 4
```

Və pointerdən istifadə edərək, biz ünvan üzrə saxlanılan dəyəri dəyişə bilərik:

```go
var x int = 4
var p *int = &x
*p = 25
fmt.Println(x)      // 25
```

Pointerləri müəyyən etmək üçün qısa formada da istifadə etmək olar:

```go
f := 2.3
pf := &f

fmt.Println("Ünvan:", pf)
fmt.Println("Dəyər:", *pf)
```

### Boş Pointer

Əgər pointerə hər hansı obyektin ünvanı mənimsədilməyibsə, o zaman həmin pointer avtomatik olaraq `nil` dəyərini alır (əslində dəyərin olmaması deməkdir). Əgər belə boş pointerin dəyərini əldə etməyə çalışsaq, xəta ilə qarşılaşacağıq:

```go
var pf *float64
fmt.Println("Dəyər:", *pf)  // ! xəta, göstərici heç bir obyektə işarə etmir
```

Buna görə, pointerlərlə işləyərkən bəzən `nil` dəyərinə qarşı yoxlama aparmaq məntiqli ola bilər:

```go
var pf *float64
if pf != nil {
    fmt.Println("Dəyər:", *pf)
}
```

### `new` funksiyası

Dəyişənlər yaddaşda adlandırılmış obyektləri təmsil edir. Go dili həmçinin adsız obyektlərin yaradılmasına imkan verir - bu obyektlər yaddaşda yerləşdirilir, lakin dəyişənlər kimi adları olmur. Bunun üçün `new(type)` funksiyasından istifadə olunur. Bu funksiyaya yaradılacaq obyektin tipi ötürülür. Funksiya həmin obyektə pointer qaytarır:

```go
package main

import "fmt"

func main() {

    p := new(int)
    fmt.Println("Dəyər:", *p)       // Dəyər: 0 - standart dəyər
    *p = 8                          // dəyəri dəyişirik
    fmt.Println("Dəyər:", *p)       // Dəyər: 8
}
```

Bu halda `p` pointer `*int` tipinə malik olacaq, çünki o, `int` tipli obyektə işarə edir. Yaradılan obyekt standart dəyərə malikdir (int tipi üçün bu 0-dır).

`new` funksiyası ilə yaradılan obyekt adi dəyişəndən heç nə ilə fərqlənmir. Yeganə fərq ondan ibarətdir ki, bu obyektə müraciət etmək üçün - ünvanı əldə etmək və ya dəyəri dəyişmək üçün pointerdən istifadə etmək lazımdır.
