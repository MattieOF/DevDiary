# Engine Selection

Before I could start actually making the game, I had to decide what engine I was going to use to make it. During the meeting, we had decided on during pure 2D - so no 3D and no 3D art made to look 2D. 

About when I started university, my engine of choice at the time, **Unity**, decided to commit to [a disastrous (and possibly illegal) rework of their licensing](https://en.wikipedia.org/wiki/Unity_(game_engine)#Runtime_fee). 
In short, they planned to charge developers for use of the engine by installation of their game, instead of a flat fee or revenue share. 

Unsurprisingly, this made a lot of developers angry, for a variety of reasons:
* Such a change should have had significant forewarning. This is the kind of thing that developers would need to prepare for. Unity planned to implement these changes in _3 months_ time.
* They tried to retroactively apply this change. Developers had signed an agreement, and now they were (possibly illegally) attempting to change this agreement retroactively.
* How installs were counted was not specified. And there didn't seem to be any good option, because:
* Bad actors could seemingly easily exploit this system by reinstalling games repeatedly (and faking networks, accounts, etc. to bypass any detections), costing developers real money.
* And finally, it's just way more complicated than it had any reason to be.

All of that is to say that I don't trust Unity anymore. Learning it any further seemed like a waste of time since it's continued use in the industry was up in the air. Instead, I considered some other popular engines:
* Unreal Engine - probably the most used engine in the world right now. Most AAA games use it, if not an in-house engine. Most definitely the highest fidelity engine, and also quite heavy-weight in general.
* Godot - a popular free and open source game engine that has seen significant attention after the Unity fiasco.
* GameMaker - a mature engine used primarily for 2D games (it has some ability to do 3D, but you pretty much have to write the renderer yourself, so just use it for 2D.)

Perhaps swepped up by the hype for the engine, I pretty quickly decided to use Godot. I didn't want to use Unreal for 
this project because it's 2D. Unreal's 2D systems were clunky in 4 and missing in 5, so Unreal is not a good choice 
for 2D games. Also, considering the low-fi art-style, it was much too heavyweight. I still believe Unreal is a great 
engine, and I enjoy using it generally, but not suited for this specific project. GameMaker also would've made sense,
but with the recent attention the engine has been getting from both indies and more established companies
([such as Mega Crit Games - the developers of Slay the Spire.](https://caseyyano.com/on-evaluating-godot-b35ea86e8cf4)),
I felt it would be more worth my time to look into learning Godot and building a portfolio for it, instead of a mature 
(and rarely used by AAA) engine such as GameMaker.

So there we go, Godot Engine. Now time to learn it.
