---
title: Pains of Golang – Generics
categories : Programming Languages
---

asdasdasd
adsasd


```go
package main

import (
	"fmt"
)

func main() {
	add := getAdd()
	jaseem(add)
}

func jaseem(add func() int) {
	fmt.Println(add())
	fmt.Println(add())
	fmt.Println(add())
}

func getAdd() func() int {
	x := 0
	return func() int {
		x++
		return x
	}
}
```