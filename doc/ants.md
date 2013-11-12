ants.go
=====
type Bot
----------------
__Description:__ Bot interface defines what we need from a bot

Declare the ```Bot``` interface
```
	type Bot interface { ... }
```
Calls the [```DoTurn()```] (/doc/MyBot.md#func-doturn) method, passing a [```State```] (/doc/ants.md#type-state) instance, optionally returns an ```error```
```
	DoTurn(s *State) error
```
__Code:__
```
	type Bot interface {
		DoTurn(s *State) error
	}
```
var stdin
----------------
Initialize ```stdin``` variable as a [bufio.NewReader] (http://golang.org/pkg/bufio/#NewReader)
```
	var stdin = bufio.NewReader(os.Stdin)
```
type State
----------------
__Description:__ Keeps track of everything that needs to be known about the state of the game

Declare the ```State``` struct
```
	type State struct { ... }
```
__List of variables:__

| var           | type        | value                               |
| ------------: | :---------: | :---------------------------------- |
| LoadTime      | ```int```   | in milliseconds                     |
| TurnTime      | ```int```   | in milliseconds                     |
| Rows          | ```int```   | number of rows in the map           |
| Cols          | ```int```   | number of columns in the map        |
| Turns         | ```int```   | maximum number of turns in the game |
| ViewRadius2   | ```int```   | view radius squared                 |
| AttackRadius2 | ```int```   | battle radius squared               |
| SpawnRadius2  | ```int```   | spawn radius squared                |
| PlayerSeed    | ```int64``` | random player seed                  |
| Turn          | ```int```   | current turn number                 |
| Map           | [```Map```] (/doc/map.md#type-map) |              |

__Code:__
```
	type State struct {
		LoadTime      int
		TurnTime      int
		Rows          int
		Cols          int
		Turns         int
		ViewRadius2   int
		AttackRadius2 int
		SpawnRadius2  int
		PlayerSeed    int64
		Turn          int
		Map *Map
	}
```
func Start
----------------
* __Description:__ Start takes the initial parameters from stdin
* __Arguments:__  none
* __Receiver:__ [```State```] (/doc/ants.md#type-state)
* __Returns:__ ```error```

```
	func (s *State) Start() error { ... }
```
Initialize ```line``` from [bufio.Reader.ReadString] (http://golang.org/pkg/bufio/#Reader.ReadString) and catch eventual ```error```
```
	line, err := stdin.ReadString('\n')
```
Check for ```error``` not null and returns it if so
```
	if err != nil {
		return err
	}
```
On empty ```line```, continue
```
	if line == "" {
		continue
	}
```
On ```line``` value equal ```ready```, exit
```
	if line == "ready" {
		break
	}
```
Initialize ```words``` as a ```slice``` (see [SplitN] (http://golang.org/pkg/strings/#SplitN))
```
	words := strings.SplitN(line, " ", 2)
```
Check that ```words``` length is greater than 2
```
	if len(words) != 2 { ... }
```
Returns an ```error```
```
	panic(`"` + line + `"`)
	return errors.New("invalid command format: " + line)
```
Initialize ```param``` as ```words[1]``` using [string conversion] (http://golang.org/pkg/strconv/#Itoa)
```
	param, _ := strconv.Atoi(words[1])
```
Switch statement on ```words[0]```
```
	switch words[0] { ... }
```
If value is ```loadtime``` set [```State.LoadTime```] (/doc/ants.md#type-state) equal ```param```
```
	case "loadtime":
		s.LoadTime = param
```
If value is ```turntime``` set [```State.TurnTime```] (/doc/ants.md#type-state) equal ```param```
```
	case "turntime":
		s.TurnTime = param
```
If value is ```rows``` set [```State.Rows```] (/doc/ants.md#type-state) equal ```param```
```
	case "rows":
		s.Rows = param
```
If value is ```cols``` set [```State.Cols```] (/doc/ants.md#type-state) equal ```param```
```
	case "cols":
		s.Cols = param
```
If value is ```turns``` set [```State.Turns```] (/doc/ants.md#type-state) equal ```param```
```
	case "turns":
		s.Turns = param
```
If value is ```viewradius2``` set [```State.ViewRadius2```] (/doc/ants.md#type-state) equal ```param```
```
	case "viewradius2":
		s.ViewRadius2 = param
```
If value is ```attackradius2``` set [```State.AttackRadius2```] (/doc/ants.md#type-state) equal ```param```
```
	case "attackradius2":
		s.AttackRadius2 = param
```
If value is ```spawnradius2``` set [```State.SpawnRadius2```] (/doc/ants.md#type-state) equal ```param```
```
	case "spawnradius2":
		s.SpawnRadius2 = param
```
If value is ```player_seed``` set [```State.PlayerSeed```] (/doc/ants.md#type-state) equal ```param64```
```
	case "player_seed":
		param64, _ := strconv.ParseInt(words[1], 10, 64)
		s.PlayerSeed = param64
```
Default value
```
	default:
		log.Panicf("unknown command: %s", line)
```
Set [```Map```] (/doc/map.md#type-map) as [```NewMap()```] (/doc/map.md#func-newmap)
```
	s.Map = NewMap(s.Rows, s.Cols)
```
Exit function
```
	return nil
```

__Code:__
```
	func (s *State) Start() error {
		for {
			line, err := stdin.ReadString('\n')
			if err != nil {
				return err
			}
			line = line[:len(line)-1]
			if line == "" {
				continue
			}
			if line == "ready" {
				break
			}
			words := strings.SplitN(line, " ", 2)
			if len(words) != 2 {
				panic(`"` + line + `"`)
				return errors.New("invalid command format: " + line)
			}
			param, _ := strconv.Atoi(words[1])
			switch words[0] {
			case "loadtime":
				s.LoadTime = param
			case "turntime":
				s.TurnTime = param
			case "rows":
				s.Rows = param
			case "cols":
				s.Cols = param
			case "turns":
				s.Turns = param
			case "viewradius2":
				s.ViewRadius2 = param
			case "attackradius2":
				s.AttackRadius2 = param
			case "spawnradius2":
				s.SpawnRadius2 = param
			case "player_seed":
				param64, _ := strconv.ParseInt(words[1], 10, 64)
				s.PlayerSeed = param64
			case "turn":
				s.Turn = param
				default:
				log.Panicf("unknown command: %s", line)
			}
		}	
		s.Map = NewMap(s.Rows, s.Cols)
		return nil
	}
```
func Loop
----------------
* __Description:__ Loop handles the majority of communication between your bot and the server. b's DoWork function gets called each turn after the map has been setup BetweenTurnWork gets called after a turn but before the map is reset. It is meant to do debugging work.
* __Receiver:__ s ([```State```] (/doc/ants.md#type-state))
* __Arguments:__  b ([```Bot```] (/doc/ants.md#type-bot)), BetweenTurnWork (```func```)
* __Returns:__ ```error```

```
	func (s *State) Loop(b Bot, BetweenTurnWork func()) error { ... }
```
Write "go\n" (see [Write()] (http://golang.org/pkg/os/#File.Write)): ___indicate we're ready___
```
	os.Stdout.Write([]byte("go\n"))
```
Initialize ```line``` from [bufio.Reader.ReadString] (http://golang.org/pkg/bufio/#Reader.ReadString) and catch eventual ```error```
```
	line, err := stdin.ReadString('\n')
```
Check for ```error``` not null
```
	if err != nil { ... }
```
Check for ```error``` equals End Of File (see [EOF])
```
	if err == io.EOF {
		return err
	}
```
Log ```error``` (see [Panicf] (http://golang.org/pkg/log/#Panicf)) and returns it
```
	log.Panicf("ReadString returns an error: %s", err)
	return err
```
___remove the delimiter___
```
	line = line[:len(line)-1]
```
Check ```line``` value is empty, if so continue
```
	if line == "" {
		continue
	}
```
Check ```line``` value equals "go"
```
	if line == "go" { ... }
```
Calls [```DoTurn()```] (/doc/MyBot.md#func-mybot)
```
	b.DoTurn(s)
```
Calls [```endTurn()```] (/doc/ants.md#func-enturn): ___end turn___
```
	s.endTurn()
```
CallBack function
```
	BetweenTurnWork()
```


__Code:__
```
func (s *State) Loop(b Bot, BetweenTurnWork func()) error {
	os.Stdout.Write([]byte("go\n"))
	for {
		line, err := stdin.ReadString('\n')
		if err != nil {
			if err == io.EOF {
				return err
			}
			log.Panicf("ReadString returns an error: %s", err)
			return err
		}
		line = line[:len(line)-1] //remove the delimiter
		if line == "" {
			continue
		}
		if line == "go" {
			b.DoTurn(s)
			s.endTurn()
			BetweenTurnWork()
			s.Map.Reset()
			continue
		}
		if line == "end" {
			break
		}
		words := strings.SplitN(line, " ", 5)
		if len(words) < 2 {
			log.Panicf("Invalid command format: \"%s\"", line)
		}
		switch words[0] {
		case "turn":
			turn, _ := strconv.Atoi(words[1])
			if turn != s.Turn+1 {
				log.Panicf("Turn number out of sync, expected %v got %v", s.Turn+1, turn)
			}
			s.Turn = turn
		case "f":
			if len(words) < 3 {
				log.Panicf("Invalid command format (not enough parameters for food): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddFood(loc)
		case "w":
			if len(words) < 3 {
				log.Panicf("Invalid command format (not enough parameters for water): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddWater(loc)
		case "a":
			if len(words) < 4 {
				log.Panicf("Invalid command format (not enough parameters for ant): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			Ant, _ := strconv.Atoi(words[3])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddAnt(loc, Item(Ant))
			if Item(Ant) == MY_ANT {
				s.Map.AddDestination(loc)
				s.Map.AddLand(loc, s.ViewRadius2)
			}
		case "A":
			if len(words) < 4 {
				log.Panicf("Invalid command format (not enough parameters for ant): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			Ant, _ := strconv.Atoi(words[3])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddAnt(loc, Item(Ant).ToOccupied())
			if Item(Ant) == MY_ANT {
				s.Map.AddDestination(loc)
				s.Map.AddLand(loc, s.ViewRadius2)
			}
		case "h":
			if len(words) < 4 {
				log.Panicf("Invalid command format (not enough parameters for ant): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			Ant, _ := strconv.Atoi(words[3])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddHill(loc, Item(Ant).ToUnoccupied())
		case "d":
			if len(words) < 4 {
				log.Panicf("Invalid command format (not enough parameters for dead ant): \"%s\"", line)
			}
			Row, _ := strconv.Atoi(words[1])
			Col, _ := strconv.Atoi(words[2])
			Ant, _ := strconv.Atoi(words[3])
			loc := s.Map.FromRowCol(Row, Col)
			s.Map.AddDeadAnt(loc, Item(Ant))
		}
	}
	return nil
}
```
meth IssueOrderRowCol
----------------
* __Description:__ Call IssueOrderRowCol to issue an order for an ant at (Row, Col)
* __Receiver:__ s ([```State```] (/doc/ants.md#type-state))
* __Arguments:__ Row (```int```), Col (```int```), d ([```Direction```] (/doc/map.md#type-direction))
* __Returns:__ _none_

__Code:__
```
func (s *State) IssueOrderRowCol(Row, Col int, d Direction) {
	loc := s.Map.FromRowCol(Row, Col)
	dest := s.Map.Move(loc, d)
	s.Map.RemoveDestination(loc)
	s.Map.AddDestination(dest)
	fmt.Fprintf(os.Stdout, "o %d %d %s\n", Row, Col, d)
}
```
func IssueOrderLoc
----------------
* __Description:__ Call IssueOrderLoc to issue an order for an ant at loc
* __Receiver:__ s ([```State```] (/doc/ants.md#type-state))
* __Arguments:__ loc ([```Location```] (/doc/map.md#type-location)), d ([```Direction```] (/doc/map.md#type-direction))
* __Returns:__ _none_

__Code:__
```
func (s *State) IssueOrderLoc(loc Location, d Direction) {
	Row, Col := s.Map.FromLocation(loc)
	dest := s.Map.Move(loc, d)
	s.Map.RemoveDestination(loc)
	s.Map.AddDestination(dest)
	fmt.Fprintf(os.Stdout, "o %d %d %s\n", Row, Col, d)
}
```
func endTurn
----------------
* __Description:__ endTurn is called by Loop, you don't need to call it.
* __Receiver:__ s ([```State```] (/doc/ants.md#type-state))
* __Arguments:__ _none_
* __Returns:__ _none_

__Code:__
```
func (s *State) endTurn() {
	os.Stdout.Write([]byte("go\n"))
}
```
