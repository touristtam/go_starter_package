MyBot.go
=====

type MyBot
---------------
Declare the ```MyBot``` struct

__Code:__
```
	type MyBot struct {}
```
func NewBots
----------------
* __Description:__ NewBot creates a new instance of your bot
* __Arguments:__ [```State```] (/doc/ants.md#type-state)
* __Receiver:__ _none_
* __Returns:__ [```Bot```] (/doc/ants.md#type-bot)

Takes a [```State```] (/doc/ants.md#type-state) Object and return a [```Bot```] (/doc/ants.md#type-bot) 
```
	func NewBot(s *State) Bot
```
Initialize a struct [```MyBot```] (/doc/MyBot.md#type-mybot)

___do any necessary initialization here___
```
	mb := &MyBot{ ... }
```
Return the instance:
```
	return mb
```

__Code:__
```
	func NewBot(s *State) Bot {
		mb := &MyBot{
		}
		return mb
	}

```
func DoTurn
---------------
* __Description:__ DoTurn is where you should do your bot's actual work.
* __Arguments:__  [```MyBot```] (/doc/MyBot.md#type-mybot)
* __Receiver:__ [```State```] (/doc/ants.md#type-state)
* __Returns:__ ```error```

Takes a [```State```] (/doc/ants.md#type-state) and pass a [```MyBot```] (/doc/MyBot.md#type-mybot) instance, optionally returns an ```error```
```
	func (mb *MyBot) DoTurn(s *State) error { ... }
```
Initialize an array of [```Direction```] (/doc/map.md#const-direction)
```
	dirs := []Direction{North, East, South, West}
```
Loop through the ```s.Maps.Ants``` as Key (```loc```), Value (```ant```)
```
	for loc, ant := range s.Map.Ants { ... }
```
Check that ```ants``` is NOT of value ```MY_ANT``` in order to discard the ```loc```
```
	if ant != MY_ANT {
		continue
	}
```
___try each direction in a random order___

Initialize ```p``` as a set of ```int``` (see [rand.Perm] (http://golang.org/pkg/math/rand/#Perm))
```
	p := rand.Perm(4)
```
Loop through the values of ```p```
```
	for _, i := range p { ... }
```
Initialize ```d``` as a [```Direction```] ()
```
	d := dirs[i]
```
Initialize ```loc2``` as a new [```Location```] (/doc/map.md#type-location) by calling [```Move()```] (/doc/map.md#func-move)
```
	loc2 := s.Map.Move(loc, d)
```
Check if destination is deemed safe by calling [```SafeDestination()```] (/doc/map.md#func-safedestination)
```
	if s.Map.SafeDestination(loc2) { ... }
```
Do the actual [```Move()```] (/doc/map.md#func-move) by calling [```IssueOrderLoc()```] (/doc/ants.md#func-issueorderloc), and end the loop
```
	s.IssueOrderLoc(loc, d)
	break
```
exit the method

___returning an error will halt the whole program!___
```
	return nil
```

__Code:__
```
	func (mb *MyBot) DoTurn(s *State) error {
		dirs := []Direction{North, East, South, West}
		for loc, ant := range s.Map.Ants {
			if ant != MY_ANT {
				continue
			}
			p := rand.Perm(4)
			for _, i := range p {
				d := dirs[i]
				loc2 := s.Map.Move(loc, d)
				if s.Map.SafeDestination(loc2) {
					s.IssueOrderLoc(loc, d)
					break
				}
			}
		}
		return nil
	}
```