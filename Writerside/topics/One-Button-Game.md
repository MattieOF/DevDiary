# Project 1: One Button Game

_In this entry, I discuss the process of creating a One Button game with an artist in an engine I've never used before: [Godot](https://godotengine.org/)!_

### Engine Selection

For my first project in the Game Development module, I have to create a game with the theme "One Button" with an artist partner. 

Since this is the first project of university, I thought I'd continue the theme of trying new things and use a new engine: Godot. There's a few reasons I decided to go with it:
- Recently, the engine has been receiving a lot of hype, especially with Unity causing outrage with the [runtime fee](https://en.wikipedia.org/wiki/Unity_(game_engine)#Runtime_fee_reception).
- The engine is meant to be very good for 2D projects (much more so than Unreal), which this one is. 
- It uses C#, which I am already familiar with.

<img alt="A reddit comment that concludes that &quot;Godot is the best 2D engine right now&quot;." height="50" src="godot_2d_reddit.png" width="400"/>

_It's not uncommon to see statements like this when researching the pros of Godot._

### Godot Experience

The first thing I created in the engine was a debug console, as the engine has no built-in way to execute commands in-game. I am used to using the console in Unreal to add variables and commands that allow easy debugging during runtime, so I wanted that in Godot too.

A feature of Godot that I haven't found in any other engine is a very easy way to make moveable UI windows, with the [Window](https://docs.godotengine.org/en/stable/classes/class_window.html) node.
It is moveable and resizeable out of the box. This saved a lot of time, so I'll definitely keep this in mind when creating UI in future.

```C#
foreach(var type in Assembly.GetExecutingAssembly().GetTypes()) 
{
    foreach (var method in type.GetMethods())
    {
        var attribute = method.GetCustomAttribute<CommandAttribute>();
        if (attribute is null)
            continue;
        
        if (!method.IsStatic)
            throw new Exception("Command functions must be static.");
        
        if (method.ReturnType != typeof(bool))
            throw new Exception("Command functions must return bool.");
        
        var methodParams = method.GetParameters();
        if (methodParams.Length == 0)
            throw new Exception("Command function must have at least one argument (of type CommandArgs).");
        if (methodParams[0].ParameterType != typeof(CommandArguments))
            throw new Exception("The first parameter of a command argument must be of type CommandArgs.");

        List<CommandArg> args = new();
        for (int i = 1; i < methodParams.Length; i++)
        {
            var param = methodParams[i];
            if (param.ParameterType != typeof(string) && !ArgConverters.ContainsKey(param.ParameterType))
                throw new Exception($"Argument \"{param.Name}\" in method {type.Name}.{method.Name} doesn't have a registered converter.");
            
            CommandArg arg;
            arg.Name = param.Name;
            arg.Typename = param.ParameterType.Name;
            arg.HasDefault = param.HasDefaultValue;
            arg.Default = param.DefaultValue;
            arg.ArgConverter = param.ParameterType != typeof(string) ? ArgConverters[param.ParameterType] : null;
            args.Add(arg);
        }

        Command cmd = new()
        {
            Method = method,
            Args = args
        };
        cmd.GenerateUsage(attribute.Names);
        foreach (string name in attribute.Names)
            _commands.Add(name, cmd);
    }
}
```

So, creating a new command is very simple. Here is some example code:

```C#
[Command("set_vsync_mode")]
public static bool VsyncMode(CommandArguments args, int mode)
{
    if (mode < 0 || mode > 3)
        args.CallingConsole.WriteError("Mode should be between 0 and 3, inclusive.");
    else
        DisplayServer.WindowSetVsyncMode((DisplayServer.VSyncMode)mode);
    return true;
}
```

The final appearance of the console is below. I'm very happy with the appearance and functionality of the console; I believe its both easy to use and easy to extend.

![kkconsole.png](kkconsole.png)

Talk about using tweens instead of manually animating a float

### Project Review

Overall, I believe this project went well for me. I learned the basics of a new engine, which is important for a programmer, as the industry is constantly changing. You need to be able to pick up new tools, and the only way to keep that ability fresh is to continually try them.

Unfortunately, communication between me and my artist broke down and I did not receive the art for the project until _after_ the deadline. In the future, I should be more proactive about receiving art and implementing it as early as possible so artists can receive feedback.

The immediate focus on creating tooling instead of gameplay definitely slowed me down in the start, and I think in future it's definitely a better idea to get gameplay prototypes up before anything like that. Regardless, the console was very helpful during the development of the game.

[Karate Kat's source code is on GitHub](https://github.com/MattieOF/OneButton/).
