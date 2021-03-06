# DSMayaTools

This is a Maya plug-in to which I intend to add any useful custom nodes or commands that I develop. Currently it only contains a single new command named __closestVertex__. This adds a command to Maya which allows you to obtain the closest vertex or face on a poly mesh to a given position in world space. I've found this handy in some skinning scripts.

There are binary builds for Maya 2013 / 15 / 16 included.

# Installation

At the moment the installation is really simple. Simply copy

> \install\\\<Maya Version\>\plug-ins\DSMayaTools.mll

to one of your Maya plugin folders. These are usually something like the paths below

> C:\Users\\\<User\>\Documents\maya\\\<Maya Version\>\plug-ins

> C:\Program Files\Autodesk\\\<Maya Version\>\bin\plug-ins

When you reload Maya load the plug-in by going to __Windows >> Settings / Preferences >> Plug-in Manager__ and ticking load next the the __DSMayaTools.mll__ entry.

# closestVertex

Once you've loaded the plug-in you should have this command available.

To report the closest vertex on a given object use the following command.

`closestVertex -face 0 -position <float x> <float y> <float z> <string object>`

To report the closest face on a given object change the value of the __face__ flag to 1.

`closestVertex -face 1 -position <float x> <float y> <float z> <string object>`

If you do not include the __face__ flag the command will work in vertex mode.

Here are some examples in MEL and Python. With two objects selected (the second has to be a polygon object) this will give the closest vertex or face on the second object to the first.

~~~~
//vertex mode
{
    string $sel[] = `ls -sl -l`;
    float $pos[] = `xform -q -ws -t $sel[0]`;
    
    int $ind = `closestVertex -p $pos[0] $pos[1] $pos[2] $sel[1]`;
    select -r ($sel[1] +".vtx[" +$ind +"]");
}

//face mode
{
    string $sel[] = `ls -sl -l`;
    float $pos[] = `xform -q -ws -t $sel[0]`;
    
    int $ind = `closestVertex -f 1 -p $pos[0] $pos[1] $pos[2] $sel[1]`;
    select -r ($sel[1] +".f[" +$ind +"]");
}
~~~~

~~~
#vertex mode
sel = cmds.ls(sl = True, l = True)
pos = cmds.xform(sel[0], q = True, ws = True, t = True)
    
ind = cmds.closestVertex(sel[1], f = False, p = pos)
cmds.select(sel[1] +'.vtx[' +str(ind) +']', r = True)

#face mode
sel = cmds.ls(sl = True, l = True)
pos = cmds.xform(sel[0], q = True, ws = True, t = True)
    
ind = cmds.closestVertex(sel[1], f = True, p = pos)
cmds.select(sel[1] +'.f[' +str(ind) +']', r = True)
~~~

# Building the Plug-in

## Pre-requisites

You'll need a recent version of CMake installed and also the correct version of Visual Studio to match the Maya version you're interested in building the plug-in for. Obviously you'll need Maya and the Maya devkit installed too.

## Build

1. Open a command prompt with the __build__ directory as your current working directory. Make sure the __build__ directory is empty. You can run __clean.bat__ in the root directory to do this.
2. Using Maya 2015 as an example run this command.

        cmake -G "Visual Studio 11 2012 Win64" -DMAYA_VERSION=2015 ../

    For other Maya versions you'll need to change the __MAYA_VERSION__ flag and also use the correct Visual Studio argument. You can see a list of the ones available by doing this.

        cmake -G
        
3. Once you've created the Visual Studio projects you can build the plugins by hand by opening the project file located in the build folder in Visual Studio or use the following command (again with the __build__ directory as the cwd).

        cmake --buid . --config Release --target Install
        
4. Once it's done the plug-in will be located in the __\install\\\<Maya Version\>__ directory.





