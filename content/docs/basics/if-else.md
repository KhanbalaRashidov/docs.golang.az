---
title: "If-Else"
description: ""
summary: ""
date: 2023-09-22T12:17:19+02:00
lastmod: 2023-09-22T12:17:19+02:00
draft: false
weight: 350
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


if və else ifadələri, Go programlaşdırma dilində, müəyyən şərtlərin doğru və ya yanlış olduğu hallarda fərqli kod bloklarının işləməsini həyata keçirir.

```go
if x > 0 {
    fmt.Println("Positive number")
} else if x < 0 {
    fmt.Println("Negative number")
} else {
    fmt.Println("Zero")
}
```

Bu nümunədə, if ifadəsi, x dəyişkənin 0 dan böyük olması halında "Positive number" mətnini yazdıracaq. Əgər x dəyişəni 0 dan böyük deyilsə, else if ifadəsi yoxlanılacaq ve x dəyişəninin 0 dan kiçik olması halında "Negative number" mətnini yazdıracaq. Əgər x dəyişəni 0 dan böyük və ya kiçik deyilsə, else bloku işləyəcək ve "Zero" mətnini yazdıracaqdır.

```go
//if və else ifadələri, qarışıq şərtləri idarə etmək üçün də istifadə oluna bilər.
if x > 10 && x < 20 {
    fmt.Println("x is between 10 and 20")
} else if x > 20 && x < 30 {
    fmt.Println("x is between 20 and 30")
} else {
    fmt.Println("x is not between 10 and 30")
}
```

Bu nümunədə, if ifadəsi, x dəyişənin 10 ve 20 arasında olması halında "x is between 10 and 20" mətnini yazdıracaqdır. Əgər x dəyişəni 10 ve 20 arasında deyilsə, else if ifadesi yoxlanılacaq və x dəyişənin 20 və 30 arasında olması halında "x is between 20 and 30" mətnini yazdıracaqdır. Əgər x dəyişəni 10 və 30 arasında deyilsə, else bloku işələyəcəkdir və "x is not between 10 and 30" mətnini yazdıracaqdır.
