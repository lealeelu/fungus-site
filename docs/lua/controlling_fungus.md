# Controlling Fungus

The Fungus module provides several functions for working with the standard Fungus narrative features and flowcharts.

You can control Say and Menu dialogs in much the same way you use Say and Menu commands in a normal Fungus flowchart. When you use the menu() function, you supply another Lua function to call when that menu option is selected. Make sure to define the function higher up in the file before referencing it in a menu() call. If you don't explicitly set a SayDialog or MenuDialog object to use, then default ones are created automatically.

# Narrative example

This example Lua script demonstrates some of the Say and Menu dialog functions. To try it out, add a LuaScript object to the scene (Tools > Fungus > Create > LuaScript) and copy this script into the Run Script text box. You'll may need to add an EventSystem object in the scene (GameObject > UI > Event System) so that the menu buttons will respond to user input.

```python
function goleft()
	say("You chose left")
end

function goright()
	say("You chose right")
end

say("Hi there")
say "This syntax also works for say commands"

menu("Go Left", goleft)
menu("Go right", goright)
```

# Say Dialog functions

To use a custom SayDialog:

1. Add as SayDialog to the scene (Tools > Fungus > Create > SayDialog)
2. Add a LuaBindings object so that Lua can access the SayDialog (Tools > Fungus > Create > LuaBindings)
3. Add a binding to the SayDialog game object, and select the SayDialog component.
4. In Lua script, you can now activate this SayDialog using the setsaydialog() function, by passing the key of the SayDialog binding.

To change the behaviour of the say() function, e.g. to not wait for input when done, do:
```python
sayoptions.waitforinput = false
```

This is the list of available functions for controlling SayDialogs.

```python
-- Options for configuring Say Dialog behaviour
sayoptions.clearprevious = true | false
sayoptions.waitforinput = true | false
sayoptions.fadewhendone = true | false
sayoptions.stopvoiceover = true | false

-- Set the active saydialog to use with the say function
-- saydialog: A binding to a SayDialog component
setsaydialog(saydialog)

-- Gets the active say dialog, or creates one if none exists yet
getsaydialog()

-- Set the active character on the Say Dialog
-- character: A Fungus.Character component
-- portrait: The name of a sprite in the character's portrait list
setcharacter(character, portrait)

-- Write text to the active Say Dialog
-- text: A string to write to the say dialog
-- voice: A voiceover audioclip to play
say(text, voiceclip)
```

# Menu Dialog functions

You setup custom MenuDialogs in the same manner as SayDialogs. This is the list of available MenuDialog functions.

```python
-- Set the active menudialog to use with the menu function
setmenudialog(menudialog)

-- Gets the active menu dialog, or creates one if none exists yet
getmenudialog()

-- Display a menu button
-- text: text to display on the button
-- callback: function to call when this option is selected
-- interactive (optional): if false, displays the option as disabled
menu(text, callback, interactive)

-- Display a timer during which the player has to choose an option.
-- duration: The length of time to display the timer.
-- callback: Function to call if the timer expires before an option is selected.
menutimer(duration, callback)

-- Clear all currently displayed menu options
clearmenu()
```

# Flowchart functions

We've added special functions for say() and menu() because these are so common in Fungus games. To execute any other commands in Fungus from Lua, you do it in conjunction with a Flowchart & Block, like this:

1. Add a Flowchart and a Block (e.g. "MyBlock") in the scene.
2. Add the Fungus commands you want to execute from Lua in the Block. (e.g Play Sounc)
3. Add a LuaBindings object (Tools > Fungus > Create > LuaBindings).
4. Add a binding to the Flowchart gameobject, and select the Flowchart component.
5. Add a LuaScript object to the scene (Tools > Fungus > Create > LuaScript)
6. Use the runblock() function to execute the Block, passing the bound flowchart and name of the block as parameters.

You can also access any Flowchart variable from Lua via the getvar() function. For example:

```python
-- Assume the 'flowchart' variable is bound to a Flowchart component in LuaBindings
-- MyVar is a string variable defined on the Flowchart

local myvar = getvar(flowchart, "MyVar") 

print(myvar.value)

myvar.value = "New value for string"
```

This is the list of available functions for controlling Flowcharts.

```python
-- Returns the specified Variable in a Flowchart.
-- To access the value of the variable, use its .value property. e.g.
--  v = getvar(flowchart, "FloatVar") 
--  v.value = 10    -- Sets the value of the variable to 10
--  f = v.value     -- f now contains 10
-- flowchart: The Fungus Flowchart containing the Block to run.
-- varname: The name of the Variable to get.
getvar(flowchart, varname)

-- Runs the specified Block in a Flowchart
-- flowchart: The Fungus Flowchart containing the Block to run.
-- blockname: The name of the Block to run.
-- nowait: If false, will yield until the Block finishes execution. If true, continue immediately.
runblock(flowchart, blockname, nowait)
```
