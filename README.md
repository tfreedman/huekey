huekey
======

Control Philips Hue lights with BetterTouchTool (or any script launcher)

Using huekey requires that you already have [phue](https://github.com/studioimaginaire/phue) installed on your system. Please go to their repository and follow the instructions there to install.

To install huekey from git you can either download the zip or execute the following command in terminal after navigating to the directory where you want huekey to live.
```
	git clone https://github.com/tfreedman/huekey
```

There are two main components to huekey: the scenes.py file and the command scripts.

##Scenes.py
You will define all of the scenes you want to be able to trigger in this file. I've provided some examples to get started, but you'll have to individually code the lights if you want to create particular scenes.
If you're uncomfortable with sending individual commands to control the lights, I'd recommend getting the status of your system when you have it set to your liking and using that response to control the lights.

Before you begin, make sure to edit the scenes.py file, line 2, to add your bridge's IP address.

You may also need to add a line immediately after line 2, containing 

```
	b.connect()
```

This is necessary if you've never used phue before, in order to get authorization from the bridge. Add b.connect(), save the file, and run ./scenes.py from the command line, after first pressing the button on your bridge. This only has to be done once, and then b.connect() can be removed. Otherwise, it's likely that your scripts will crash, as they don't have access to the Hue API.

For example, if I like the settings on lights 2 and 3 and want to turn that into a scene, I would execute the following commands in Terminal:
```
	python
	from phue import bridge
	b = Bridge('Your.IP.Address.Here') #phue will attempt to establish a connection with the bridge here. If this is the first time running it, you'll need to press the button on the bridge
	b.get_light(2)['state'] #copy the response from this command and add it to the scenes.py file using a text editor.
	b.get_light(3)['state'] #copy the response from this command and add it to the scenes.py file using a text editor.
```
Now the preset for lights 2 and 3 has been discovered and added to the scenes.py file. Now I need to take the scenes and turn them into something BetterTouchTool (BTT) can see.

##Command scripts
Open up a command script (we'll use example.command, but alloff.command and nightlight.command have also been provided) in a text editor. These files are pretty basic but should help keep scene activation run quickly.
The command files contain the following three lines (we'll use the example file):
```
	#!/usr/bin/env python  # <--This will tell terminal that the following lines should be executed in python.
	from scenes import example  # <--This imports the example scene from the scenes.py file.
	example() # <--This runs the example scene and changes the lights.
```
After you change the above lines to work, save the file with a name that you'll remember.

**IMPORTANT:** The command scripts need to be in the same folder as the scenes.py file to work. They also need to be executable (Either via the Terminal by running: chmod +x file.command or by Get Info in Finder)

##Creating AppleScripts
BetterTouchTool has the ability to run a script when you press a keyboard shortcut. However, if you run a bash script / terminal command, a terminal window will pop open and close every time you press a key. The workaround to this is to create an AppleScript that runs the terminal command scripts created above, as AppleScripts don't spawn windows when they run.

Open the AppleScript Editor, and paste the following:

```
	do shell script "/Users/YOUR.NAME.HERE/Applications/HueKey/YOUR.COMMAND.HERE.command"
```

Press compile, then save it in the same folder as the .command files / scenes.py file. 

##BetterTouchTool
Now for the final step. Open BTT and create your actions, pointing the triggers to "Open Application / File / Script" and then selecting the AppleScript you want to run. Then test it and it should work!

If you didn't create an AppleScript, you can also select the .command files directly. However, this will open and close a terminal every time, which isn't as clean.

##Other Apps
Not satisfied with BTT, and you'd rather activate these with a keyboard command? Take a look at [FastScripts](http://www.red-sweater.com/fastscripts/) or any other app that'll let you launch a script with a keyboard shorcut (Keyboard Maestro comes to mind, or Alfred...).
