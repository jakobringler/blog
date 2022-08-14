---
title: "Scripting in Houdini - Talk Notes"
tags:
- houdini
- python
---

These are my personal notes about Paul Ambrosiussen's Talk [Python Scripting - Houdini](https://www.youtube.com/watch?v=CxoVzsxiruY&t=2605s).

### Writing Code

use an external editor for anything longer than a couple of lines

##### Node Parameter Editor
- worst way
- limited space
- copy pasting to external editors for improved python features is prone to errors

##### Expression Editor
- okayish
- more space
- limited editor features
- same copy pasting problem
- useful in PDG >> displays most common pdg python functions in a column

##### External Editor
- install SideFX Labs to be able to link an external editor (Sublime, VS Code etc.) to write code
- VS Code has a VEX language extension to enable auto complete

---

### Raising Errors

```Python
error = "Error!"

if not something:
	raise hou.NodeError(error)
```

### Raising Warnings

```Python
warning = "Warning?"

if not something:
	raise hou.NodeWarning(warning)
```

### Raising Messages

```Python
message = "Message."

if not something:
	raise hou.NodeMessage(message)
```

---

### Run, Execute & Communicate with Thrid Party Applications

##### Subprocess

library that ships with Python and lets you run other programs

```Python
import subprocess
```

##### CMD Strings

To run another program you have to build a command (cmd) string. Essentially a single commandline snippet that runs some executable with all the necessary parameters.

```Python
executablepath = "path/to/program.exe"
value = 5 # some value from Houdini

cmd = [executablepath]

if something:
	cmd.append("-p") # append a parameter
	cmd.append(value) #parameter value
```

This results in the following cmd string: `path/to/program.exe -p 5`

##### Running the Subprocess

```Python
# disable IM dialog box (windows specific)
si = None
if os.name == 'nt':
	si = subprocess.STARTUPINFO()
	if hide_instant_meshes_dialog:
		si.dwFlags != subprocess.STARTF_USESHOWWINDOW

subprocess.call(cmd, startupinfo=si)
```

In case you want to get some geometry back this function overwrites any input geo and returns the new one instead.

```Python
geo.loadFromFile(path)
```

### Run Code Based on UI Events

possible using Parameter Callbacks




---

sources / further reading:
- [Python Scripting - Houdini - Paul Ambrosiussen](https://www.youtube.com/watch?v=CxoVzsxiruY&t=2605s)


