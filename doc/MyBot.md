MyBot
=====


func NewBots
----------------
Takes a ```State``` Object and return a ```Bot```:
```
	func NewBot(s *State) Bot
```
Initialize a struct ```MyBot```:
```
	mb := &MyBot{}
```
Return the instance:
```
	return mb
```
Complete method:
```
	func NewBot(s *State) Bot {
		mb := &MyBot{
		}
		return mb
	}

```
func DoTurn
---------------
Takes a ```State``` Object and apply it to a ```MyBot``` instance, optionally returns an error
```
	func (mb *MyBot) DoTurn(s *State) error
```
Initialize an array of ```Direction```
```
	dirs := []Direction{North, East, South, West}
```
Loop through the ```s.Maps.Ants``` as Key (```loc```), Value (```ant```)
```
	for loc, ant := range s.Map.Ants
```
Check that ```ants``` is NOT of value ```MY_ANT``` in order to discard the ```loc```
```
	if ant != MY_ANT {
		continue
	}
```
Initialize ```p``` as a set of ```int``` (see [rand.Perm] (http://golang.org/pkg/math/rand/#Perm))
```
	p := rand.Perm(4)
```
Loop through the values of ```p```
```
	for _, i := range p
```
Initialize ```d``` as a Direction
```
	d := dirs[i]
```
Initialize ```loc2``` as a new Location (could have been called __destination__ in this context)
```
	loc2 := s.Map.Move(loc, d)
```

Complete method:
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