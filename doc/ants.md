ants
=====
type Bot
----------------
Declare the ```Bot``` interface

Defines what we need from a bot
```
	type Bot interface 
```
Calls the [```DoTurn()```] (/doc/MyBot.md#func DoTurn) method, passing a [```State```] (/doc/ants.md#type State), optionally returns an ```error```
```
	DoTurn(s *State) error
```
Complete definition
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
Declare the ```State``` struct

Keeps track of everything that needs to be known about the state of the game

| var           | type        | value                               |
| :------------ | :---------: | :---------------------------------: |
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
Takes a [```State```] (/doc/ants.md#type State) Object, optionally returns an error
```
	func (s *State) Start() error 
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
Complete method:
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