# Project 2: Farm Survivors

_In this entry, I discuss reuse of code, and using data assets to bring design decisions out of code._

### Toolset

For this project, I reused the console I had made for the previous project. This allowed me to use the console
for the same productivity-accelerating purposes as I had previously, without having to develop the console 
itself. 

I was able to add commands specific to this project very easily thanks to how I had programmed the system.
For example, to kill all enemies:

```C#
[Command("kill_all")]
public static bool KillAll(CommandArguments args)
{
    var enemies = args.Tree.GetNodesInGroup("Enemy");
    foreach (var enemy in enemies)
        ((Enemy)enemy).Die();

    args.CallingConsole.WriteLine(enemies.Count == 0 ? "No enemies to kill!" : $"Killed {enemies.Count} enemies. You monster.");
    return true;
}
```

Or to print out a players inventory:

```C#
[Command("print_inv")]
public static bool PrintInventory(CommandArguments args)
{
    var player = args.Tree.GetFirstNodeInGroup("Player") as Player;

    if (player is null)
    {
        args.CallingConsole.WriteError($"No player currently exists!");
        return false;
    }

    if (player.Inventory._inv.Count == 0)
    {
        args.CallingConsole.WriteLine($"The players inventory is empty.");
        return true;
    }
    
    args.CallingConsole.WriteLine("Inventory:");	
    foreach (var item in player.Inventory._inv)
    {
        args.CallingConsole.WriteLine($"	{item.Key.Name}: {item.Value}");	
    }
    
    return true;
}
```

This was super useful during development, and has made me consider creating a backlog of development tools that
I can bring to each project I work on. 

### Data-Driven Content

One piece of feedback I received during development is that it's hard for designers to test changes when
all the related values are hardcoded in game code.

My solution to this problem would be to utilise data and assets instead of code. In Unity, I had previously
used [Scriptable Objects](https://docs.unity3d.com/Manual/class-ScriptableObject.html) to achieve a similar goal.
Through [research](https://www.reddit.com/r/godot/comments/16kckej/godots_take_on_handling_a_scriptable_object/), I found that Godot's method for this is to use [custom Resources](https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html):

![custom_resource_research.png](custom_resource_research.png)

I could then create a subclass of Resource for each type of data I wanted to store. For an example, data for an item type:

![itemtypedata.png](itemtypedata.png)

I can then create and edit assets of this type to store data for a certain type of Item:

<video src="../videos/fsdataexample.mp4"/>

There are multiple benefits to this method:
- These resources can be provided as parameters very easily
- The data could be changed without a relatively slow code recompile
- Designers could edit the data without any code knowledge

I found this solution increases productivity, allows easier references to assets like textures,
and allows for easier collaboration with team members not experienced with programming.

### Feedback

One piece of feedback I received is extremely poor performance once the bridge is enabled.
After some research, I believe it may be related to my approach to navigation.
[This GitHub issue](https://github.com/godotengine/godot/issues/66738) describes how performance
will be poor when using baked navigation on TileMaps, and asking for a path that is impossible (which may be happening with the bridge.)

In the future, I may look into using NavigationMeshInstances as the post suggests, or create a custom navigation and movement
implementation that is optimised for _many_ characters.
This was after the deadline, so I did not implement these changes. 
