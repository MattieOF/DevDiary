# Project 2: Farm Survivors

_In this entry, I discuss reuse of code, and using data assets to bring design decisions out of code._

### Toolset

Talk about using previously made console. Interest in developing a suite of tools for future projects

### Data-Driven Content

One piece of feedback I received during development is that it's hard for designers to test changes when
all the related values are hardcoded in game code.

My solution to this problem would be to utilise data and assets instead of code. In Unity, I had previously
used [Scriptable Objects](https://docs.unity3d.com/Manual/class-ScriptableObject.html) to achieve a similar goal. 

<video src="../videos/fsdataexample.mp4"/>

### Conclusion

One piece of feedback I received is extremely poor performance once the bridge is enabled.
After some research, I believe it may be related to my approach to navigation.
[This GitHub issue](https://github.com/godotengine/godot/issues/66738) describes how performance
will be poor when using baked navigation on TileMaps, and asking for a path that is impossible (which may be happening with the bridge.)

In the future, I may look into using NavigationMeshInstances as the post suggests, or create a custom navigation and movement
implementation that is optimised for _many_ characters.
This was after the deadline, so I did not implement these changes. 
