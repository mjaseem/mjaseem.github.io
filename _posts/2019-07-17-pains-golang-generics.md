---
title: Pains of Golang – Generics
categories : 'ProgrammingLanguages'
---

asdasdasd
adsasd


```go
func DoNAtATime(op interface{}, arr interface{}, n int) {
	arrValue := reflect.ValueOf(arr)
	arrType := arrValue.Type()
	fn := reflect.ValueOf(op)
	fnType := fn.Type()
	
	if fnType.Kind() != reflect.Func || fnType.NumIn() != 1 || fnType.NumOut() != 0 {
		log.Panic("Expected a unary function returning nothing")
	}
	if arrType.Kind() != reflect.Slice || arrType.Elem() != fnType.In(0) {
		log.Panic("arr type and op argument type are mismatched")
	}
	
	invokeFnOnArrayValue(arrValue, fn)
}

func invokeFnOnArrayValue(arrValue Value, fn Value){
	sem := make(chan int, n) //Use buffered chan as counting semaphor
	var wg sync.WaitGroup
	for i := 0; i < arrValue.Len(); i++ {
		sem <- 1
		item := arrValue.Index(i)
		wg.Add(1)
		go func(item reflect.Value) {
			fn.Call([]reflect.Value{item})
			<-sem
			wg.Done()
		}(item)
	}
	wg.Wait() //using waitgroup to block until completion
}
```