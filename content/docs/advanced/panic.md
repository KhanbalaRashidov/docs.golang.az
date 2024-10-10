---
title: "Panic"
description: ""
summary: ""
date: 2023-09-22T12:17:19+02:00
lastmod: 2023-09-22T12:17:19+02:00
draft: false
weight: 989
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---


`panic`, Go dilində bir proqram işləmə zamanı gözlənilməz bir vəziyyətlə qarşılaşdıqda, proqramın icrasını dayandıraraq bir xəta mesajı göstərməsini təmin edən xüsusiyyətdir.

`panic` ciddi səhvləri idarə etmək üçün istifadə edilməlidir və mümkün qədər qarşısını almağa çalışılmalıdır. Lakin, kritik bir xəta baş verdikdə, proqramın dərhal dayandırılması lazım olan vəziyyətlərdə `panic` faydalı ola bilər.

### `panic` istifadə nümunəsi:

```golang
package main

import "fmt"

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Panic yakalandı:", err)
		}
	}()

	divideByZero()
	fmt.Println("Bu kod çalışacak mı?")
}

func divideByZero() {
	numerator := 10
	denominator := 0
	result := numerator / denominator
	fmt.Println("Sonuç:", result)
}
```

Bu nümunədə, `divideByZero` adlı bir funksiya var. Bu funksiya daxilində `numerator` 10 və `denominator` 0 olaraq təyin edilir, daha sonra `result` dəyişəninə `numerator`u `denominator`ə bölürük. Lakin, `denominator`un 0 olması vəziyyətində, sıfıra bölmə xətası baş verir və bu, `panic` vəziyyətini yaradır.

### **Açıqlama**:

1. **Panic və recover istifadəçi:**
   `defer` istifadə edərək bir funksiyanı gecikdiririk, burada `recover()` funksiyası `panic` vəziyyətlərini ələ keçirir. Əgər `panic` vəziyyəti yaranarsa, `recover()` həmin `panic` səbəbini tapır və işləyir.

2. **Sıfıra bölmə xətası:**
   `divideByZero()` funksiyasında sıfıra bölmə cəhdi olur, bu isə `panic` vəziyyətinə səbəb olur. Lakin, `recover()` bu `panic`i ələ keçirir və proqramın dayanmadan davam etməsinə imkan yaradır.

3. **Nəticə:**
   Proqram bu vəziyyəti tutaraq "`Panic yakalandı: runtime error: integer divide by zero`" kimi bir mesaj göstərir və proqram sonrakı kodu icra etməyə davam edir.

Bu yanaşma proqramların gözlənilməz xətaları idarə etməsinə və stabil qalmasına kömək edir.
