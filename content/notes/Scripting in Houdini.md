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

### Raising Errors, Warnings & Messages
Errors and Warnings are displayed in the node's info section while Messages trigger a popup window 

```Python
error = "Error!"

if not something:
	raise hou.NodeError(error)
```

```Python
warning = "Warning?"

if not something:
	raise hou.NodeWarning(warning)
```

```Python
message = "Message."

if not something:
	raise hou.ui.displayMessage(message)
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
to run the command you can use the `call()` function

```Python
# disable IM dialog box (windows specific)
si = None
if os.name == 'nt':
	si = subprocess.STARTUPINFO()
	if hide_dialog:
		si.dwFlags != subprocess.STARTF_USESHOWWINDOW

subprocess.call(cmd, startupinfo=si)
```

In case you want to get some geometry back this function overwrites any input geo and returns the new one instead.

```Python
geo.loadFromFile(path)
```

### Run Code Based on UI Events
[Relevant Doc Pages](https://www.sidefx.com/docs/houdini/hom/locations.html#asset_modules)

When building HDAs it's possible to use python callbacks to trigger scripts with custom buttons or on events such as `OnCreated()` or `OnInputChanged()`. For a full list of events have a look at the [DOCs](https://www.sidefx.com/docs/houdini/hom/locations.html#asset_events).

### Shelf Tools
Easiest way is to select a node tree and drag and drop the nodes on an empty shelf spot. this will automatically create all the necessary script to recreate the setup on the push of the shelf button. Unfortunately this creates quite messy and bloated code.

To write it yourself you can start by creating a 'New Tool' on a shelf and importing the hou module. To easily find all the necessary functions, and python objects you can drag and drop nodes inside the scripting panel which will give you the correct syntax to access the data.

### Pre and Post Save
you can trigger scripts before and after a hip file is saved adding code to `HOUDINIPATH/scripts/beforescenesave.py & afterscenesave.py` -> [DOCS](https://www.sidefx.com/docs/houdini/hom/locations.html#run-scripts-before-and-or-after-saving-the-scene-hip-file)

### Processing Node Network / Script Analysis
Counting how often each node gets used (useful to figure out support and update importance in bigger studios):

```Python
import hou
from collections import Counter

parentnode = hou.node("/obj/geo1")
allnodes = parentnode.children()

nodenames = [x.type().name() for x in allnodes]

print(Counter(nodenames))
```

### Dropdown Menus
Every Houdini Menu is described by a .xml file that describes how it's constructed and what items it consists of. Take a look at SideFX Labs `MainMenuCommon.xml`.

Python Scripts can be added in an xml in the following way:

```xml
<submenu id="name_menu">
	<scriptItem id="itemid">
		<label>LABEL</label>
		<insertAtIndex>10</insertAtIndex>
		<scriptCode>
			<![CDATA[
			PYTHONSCRIPT
			]]>
		</scriptCode>
	</scriptItem>
</submenu>
```

### Startup Scripts
can be used to sanity check or load default scenes etc. on startup

`pythonrc.py` runs on startup (as many as found in $HOUDINIPATH)
`123.py` runs when Houdini is started without a scene (only first found)
`456.py` runs after a scenefile is loaded (only first found)


---

sources / further reading:
- [Python Scripting - Houdini - Paul Ambrosiussen](https://www.youtube.com/watch?v=CxoVzsxiruY&t=2605s)


