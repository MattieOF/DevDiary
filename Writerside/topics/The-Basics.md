# The Basics

The first thing any good game starts with is an empty project. However, the first thing any good empty project starts with is an installation of the engine, so let's get one of those first.

### Downloading the Engine
 
The engine can, of course, be downloaded from [the Godot website](https://godotengine.org/download/windows/).
The first thing I noticed is that the engine is only a 60MB download. Compared to [Unreal's 115GB storage requirement](https://catnessgames.com/blog/unreal-engine-5-equirements/), 
this reflects Godot's focus on being lightweight, which is exactly what I wanted for this project.

![Godot takes up less than 150MB when fully extracted.](../images/GodotFileSize.png)

With the engine installed, I could launch the engine, and create a project (for now called "OneButton" since we don't know what to call the game yet).

### Godot Architecture
Before I could start working in the engine, I had to understand the core architecture of the engine. 
With previous experience in engines like Unity and Unreal, I expect to be able to apply my current knowledge to Godot while just learning the engines idiosyncrasies. 
It's a bit like learning a new programming language while already having years of experience with programming - you don't need to learn how to program, just the syntax of the new language. 

I plan to learn this through [Godot's early but fast-moving documentation](https://docs.godotengine.org/en/stable/about/introduction.html).
For example, the documentation contains [an explainer for Godot's key concepts](https://docs.godotengine.org/en/stable/getting_started/introduction/key_concepts_overview.html), which is exactly what I need to learn what I need to learn.
I'll write about these key concepts here, if anything to further my understanding of them.

In Godot, everything within the game is a _Node_ or a _Resource_. Every sprite, collision box, particle (system), UI text, instance of a 3D model, sound effect player, and even the game window itself are Nodes. 
As a rule of thumb, anything placed within the world is a Node at its core. 
A resource is any asset or other object that is used throughout the project in multiple instances. For example, any 3D model, texture, or C# script is a Resource.

This architecture is common throughout game engines. For example, a Node in Godot is comparable to a GameObject in Unity. 
One difference is that Nodes do _not_ have components. They are subclasses of another type of Node or the base Node class itself. 
All functionality for a specific node is defined by that subclass - there is no way to append functionality to a specific node itself. 
Instead, more complex functionality is achieved via child Nodes (so, a component in Unity can be compared to a child Node in Godot).

[TALK ABOUT SCENE TREE]

[TALK ABOUT SCENES]

### The First Scene(s)

[CREATE A PLAYER SCENE, ADD SPRITE]

[IMPORT SPRITE]

[CREATE A ROOT SCENE, SET AS DEFAULT]

With the basics of Godot down, I should move on to implementing the game's core mechanics: a... console window?
