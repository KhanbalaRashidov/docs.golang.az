---
title: "Timers"
description: ""
summary: ""
date: 2023-09-22T12:17:19+02:00
lastmod: 2023-09-22T12:17:19+02:00
draft: false
weight: 970
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Go dilində timer-lər, müəyyən bir müddət keçdikdən sonra bir əməliyyatın yerinə yetirilməsini təmin etmək üçün istifadə olunur. `time` paketi daxilindəki `NewTimer` funksiyası ilə timer yaradıla bilər.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	timer1 := time.NewTimer(2 * time.Second)

	<-timer1.C
	fmt.Println("Timer 1 expired")

	timer2 := time.NewTimer(time.Second)

	go func() {
		<-timer2.C
		fmt.Println("Timer 2 expired")
	}()

	stop2 := timer2.Stop()
	if stop2 {
		fmt.Println("Timer 2 stopped")
	}
}
```

Bu nümunədə:

* `NewTimer` funksiyası ilə iki timer yaradılır.
* Birinci timer (`timer1`) 2 saniyədən sonra bitəcək şəkildə təyin olunur. Timer bitdikdə `<-timer1.C` ilə gözlənilir və "Timer 1 expired" mesajı ekrana yazdırılır.
* İkinci timer (`timer2`) 1 saniyədən sonra sona çatacaq. `goroutine` ilə bu timer izlənir və əgər timer vaxtı bitərsə, "Timer 2 expired" mesajı çıxar. Lakin `Stop` funksiyası ilə bu timer vaxtı dolmadan dayandırılır və "Timer 2 stopped" mesajı ekrana yazdırılır.

#### Output:

```go
Timer 1 expired
Timer 2 stopped
```

Bu nümunədə, timer-lərdən biri müəyyən müddətdən sonra bitir və bir əməliyyat yerinə yetirilir, digəri isə vaxtı dolmadan əvvəl `Stop` funksiyası ilə dayandırılır. Timer-lərin bu cür idarə olunması zamanlama əməliyyatlarının nəzarətində faydalıdır.
