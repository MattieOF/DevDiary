# Project 1: Karate Kat

_In this entry, I discuss the One Button project, the creation of a console utility, and tweening._

### Engine Selection

For my first project in the Game Development module, I have to create a game with an artist. 

Since this is the first project of university, I thought I'd continue the theme of trying new things and use a new engine: Godot. There's a few reasons I decided to go with it:
- Recently, the engine has been receiving a lot of hype, especially with Unity causing outrage with the [runtime fee](https://en.wikipedia.org/wiki/Unity_(game_engine)#Runtime_fee_reception).
- The engine is very good for 2D projects (much more so than Unreal).
- It uses C#, which I am already familiar with.

It's not uncommon to see statements like this when researching the pros of Godot:

<img alt="A reddit comment that concludes that &quot;Godot is the best 2D engine right now&quot;." height="50" src="godot_2d_reddit.png" width="400"/>

### Console

The first thing I created in the engine was a debug console, as Godot has no built-in way to execute commands in-game.
I am used to using the console in Unreal to add variables and commands that allow easy debugging during runtime, so I wanted that in Godot too.

A feature of Godot that I haven't found in any other engine is a very easy way to make moveable UI windows, with the [Window](https://docs.godotengine.org/en/stable/classes/class_window.html) node.
It is moveable and resizeable out of the box. This saved a lot of time, so I'll definitely keep this in mind when creating UI in future.

Originally, the code required each command to be registered manually, providing a name and a function to process the command.
However, in Unreal, commands are created using the ``Exec`` function specifier. This is a form of metadata.
It allows creating commands to be an extremely user-friendly experience; the system handles everything for you, 
and you just have to write the functionality of the command.  There is no need to add code elsewhere to register the command. 
This is behaviour I really wanted to achieve.

I found [an article](https://medium.com/@lexitrainerph/mastering-c-attributes-a-comprehensive-guide-from-basics-to-advanced-38322b54dd98) 
that how to use [Attributes](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/), which I realised I could use to achieve something similar in C#.

I go through all static functions in my games code, looking for commands.
One interesting part of this code is how we check for default values, so that the user can easily 
use native language features to specify aspects of the command.

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

One issue with this is that it works using C# specific features. If I wanted to achieve something siliar in C++, I would
not have reflection features to utilise, and would instead have to rely on static initialisation or, worse, boilerplate code
to achieve the same thing.

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

I'm very happy with the appearance and functionality of the console; 
I believe it's both easy to use and easy to extend, which is exactly what I was aiming for.

![ConsoleDemo.gif](ConsoleDemo.gif)

### Tweening

Since the design was simple, the gameplay was fairly easy to implement, and mainly just required lots of referring to the [Godot Documentation](https://docs.godotengine.org/en/stable/index.html) to translate my Unity/Unreal experience to Godot.

One thing I did learn during the project was using _tweens_. Tweens allow me to smoothly interpolate values with just a couple lines of one-shot code.
Previously, I would create float variables, update them every frame, and use them in equations to make animations in code.
This is a lot of setup, and requires adding a tick function to an object that may otherwise not need one, hurting performance.
In Godot, I could use the built-in tweening system to modify values very easily:

```C#
var tween = helpText.CreateTween();

tween.TweenProperty(helpText, new NodePath(Control.PropertyName.Position),
 Variant.From(new Vector2(1200, helpText.Position.Y)), .5f).SetEase(Tween.EaseType.Out);
 
tween.TweenProperty(cat, new NodePath(Sprite2D.PropertyName.Position),
 Variant.From(catStart), .5f).SetEase(Tween.EaseType.Out);
```

I also experimented with the ``SetEase`` function, that allows me to add smoothing to the animations to make
them feel much nicer from a game feel perspective.

Tweening can modify any value of a lerpable type. For example, we can tween the pitch of the music to add effect when the player dies:

```C#
// Check if the music tween is active. 
// If it is, stop it first to avoid overlapping tweens.
if (_musicTween is not null && _musicTween.IsRunning())
    _musicTween.Stop();

_musicTween = music.CreateTween();
_musicTween.TweenProperty(music, new NodePath(AudioStreamPlayer.PropertyName.PitchScale), 0.1, 0.5f);
```

<video src="../videos/kkmusicexample.mp4"/>

Tweening is a very useful tool that saves lots of boilerplate code while remaining performant,
and offering useful functions for improving the feel of animations.
In the future, I'll keep this in mind when creating animations in Godot code, and will seek out similar 
features in other engines.

### Project Review

Overall, I believe this project went well for me. I learned the basics of a new engine, which is important 
for a programmer, as the industry is constantly changing. You need to be able to pick up new tools, and the only 
way to keep that ability fresh is to continually try them.

Unfortunately, communication between my artist and I broke down and I did not receive the art for 
the project until _after_ the deadline. In the future, I should be more proactive about receiving art
and implementing it as early as possible so artists can receive feedback.

The immediate focus on creating tooling instead of gameplay definitely slowed me down in the start, 
and I think in future it's definitely a better idea to get gameplay prototypes up first. 
If I spent more time on those, I may have been able to create a more interesting design.
