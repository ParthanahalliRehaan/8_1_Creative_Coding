# D1, can we grab a child node of a different parent in GoDot?
Concise reason:  
`$nodename` only works for children of the current node. It cannot reach nodes under a different parent.  

Concise solution:  
Use `get_node()` with a relative or absolute path. For example, if you’re inside `Player` and want `Weapon` under `Enemy`:

```gdscript
@onready var weapon = get_node("../Enemy/Weapon")
```
# D2, How to connect to a signal using GDScript in GoDot?
```gdscript
extends Node

func _ready():
    # Connect a built-in signal
    $Button.pressed.connect(_on_button_pressed)

func _on_button_pressed():
    print("Button was pressed!")
```

### Key points:
- Use `.connect()` on the signal → `node.signal.connect(function_name)`
- The function you connect must match the signal’s expected parameters
- To stop listening: `node.signal.disconnect(function_name)`
