---
title: "Slices"
description: ""
summary: ""
date: 2023-09-22T12:17:19+02:00
lastmod: 2023-09-22T12:17:19+02:00
draft: false
weight: 370
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


Dilimin əsas məlumat strukturlarından biri olan slices, Go proqramlaşdırma dilində ölçüsü dəyişdirilə bilən, elastik bir array məlumat strukturdur. Slices, array-lərin bir alt hissəsi kimi qəbul edilə bilər və Go dilində tez-tez istifadə olunur.

```go
a := []int{1, 2, 3, 4, 5}
fmt.Println(a)
```

Bu nümunədə, a adlı bir dilim təyin edilir və elementləri {1, 2, 3, 4, 5} olaraq təyin edilir. Array-lərdən fərqli olaraq, dilimlərin ölçüsü təyin edilmə mərhələsində göstərilmir. Slices boş da təyin edilə bilər.

```go
var a []int
a = append(a, 1)
a = append(a, 2, 3, 4, 5)
fmt.Println(a)
```

Bu nümunədə, a adlı bir dilim təyin edilir və append() funksiyası ilə elementlərinə müvafiq olaraq 1, 2, 3, 4 və 5 dəyərləri əlavə edilir. append() funksiyası, bir dilimə bir və ya bir neçə element əlavə etmək üçün istifadə olunur.

Dilimlər, array-lərdən fərqli olaraq, bir alt hissə kimi də təyin edilə bilər.

```go
a := []int{1, 2, 3, 4, 5}
b := a[1:4]
fmt.Println(b)
```

Bu nümunədə, a adlı bir dilim təyin edilir və elementlərinə {1, 2, 3, 4, 5} dəyərləri verilir. b adlı bir dilim isə a diliminin 1-ci indeksindən 4-cü indeksinə qədər olan alt hissəsini əhatə edir. b dilimindəki elementlər {2, 3, 4} olaraq göstərilir.
