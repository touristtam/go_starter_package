main
=====

func main
----------------
Initialize a ```State```
```
	var s State
```
Call method [```s.Start()```] (/doc/ants.md#func Start) and Initialize an ```error``` var
```
	err := s.Start()
```
Check for error
```
	if err != nil
```
If so log it
```
	log.Panicf("Start() failed (%s)", err)
```
Initialize a [new bot] (/doc/MyBot.md#func NewBot) by passing the ```State```
```
	mb := NewBot(&s)
```
Call method ```s.Loop()``` and get eventual ```error```
```
	err = s.Loop(mb, func() { ... })
```
Check for error (see [EOF] (http://golang.org/pkg/io/#pkg-variables))
```
	if err != nil && err != io.EOF
```
If so log it
```
	log.Panicf("Loop() failed (%s)", err)
```
Complete method:
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