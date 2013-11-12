main.go
=====

func main
----------------
__Description:__ Initializes the state and starts the processing loop

Initialize a [```State```] (/doc/ants.md#type-state)
```
	var s State
```
Call method [```s.Start()```] (/doc/ants.md#func-start) and Initialize an ```error``` var
```
	err := s.Start()
```
Check for error
```
	if err != nil { ... }
```
If so log it
```
	log.Panicf("Start() failed (%s)", err)
```
Initialize a [```NewBot```] (/doc/MyBot.md#func-newbot) by passing the [```State```] (/doc/ants.md#type-state)
```
	mb := NewBot(&s)
```
Call method [```s.Loop()```] (/doc/ants.md#func-loop) and get eventual ```error```

___If you want to do other between-turn debugging things, you can do them here___
```
	err = s.Loop(mb, func() { ... })
```

Check for error (see [EOF] (http://golang.org/pkg/io/#pkg-variables))

```
	if err != nil && err != io.EOF { ... }
```
If so log it
```
	log.Panicf("Loop() failed (%s)", err)
```

__Code:__
```
	func main() {
		var s State
		err := s.Start()
		if err != nil {
			log.Panicf("Start() failed (%s)", err)
		}
		mb := NewBot(&s)
		err = s.Loop(mb, func() {
		})
		if err != nil && err != io.EOF {
			log.Panicf("Loop() failed (%s)", err)
		}
	}

```