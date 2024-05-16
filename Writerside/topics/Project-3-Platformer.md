# Project 3: Steampunk Platformer

_In this entry, I discuss how source control enabled collaboration, saved the entire project, and how I might improve my usage in the future._

### Collaboration via Source Control

While I had been using source control via GitHub for a couple of years at the time of
starting the course, I had never used source control to collaborate with other people.
Thanks to a Games Development lecture, my team and I were aware of how to share a Git
repository with other team members. I got the GitHub usernames of all team members and 
invited them to join the repository.

One example of how this helped our process was collaborating on character movement. Originally,
I set up a basic character with only slight changes to the default values, and added some extra
mechanics, such as dash. I then committed these changes, and directed my designer on how to load
the project and tweak the values until they felt right. He then pushed his changes, and I was able 
to work on the values further based on feedback until we thought it was right.

![platformerjamcollab.png](platformerjamcollab.png)

This process made collaborating remotely seemless. 
In comparison to previous methods (manually zipping, sending, editing, and resending game files), 
it's an order of magnitude more productive.
In the future, I'll definitely add every team member to the source repository 
so that they can make changes and easily see the current state of the project.

Source control also ended up saving the entire project! Only a couple of days before the project was due, my laptop died. Some sort of electrical short. It no longer turned on, and for all I knew, all the data on it was lost.
Due to some warranty fine print, I wouldn't be getting it fixed for a whole month. 

If I hadn't used source control, it's possible the entire game would've been lost due to this, and we would've ended up with literally nothing.
Thankfully, I had, so while I did loose some un-pushed code, I was able to recover most of the project and get my part done in time.

This taught me that source control is an invaluable tool, and I plan to use it for every single project; from game jams to commercial projects. 
Furthermore, it's very useful for fostering collaboration - it allows 
I think in future I should be more proactive about commiting and pushing - I would only commit every few hours, and this meant that:

- The changes per-commit were too large, making it hard to make a succinct commit title and description
- When my PC died, I lost more code than I should've, since much of it hadn't been committed yet.

Using [branches](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches) so that I can push in-progress code (that may not even compile!) without breaking the main branch would be a good solution to this.

![platformer-gittree.png](platformer-gittree.png)

![imgui-gittree.png](imgui-gittree.png)

For example, above are two examples of a git tree. Each line on left represents a branch. 
The first one is the project we've been talking about (I created the "pre-tragedy" branch later, once I was able to recover the code left on the dead laptop).
The second is from an established open-source project, [im-gui](https://github.com/ocornut/imgui).

In the im-gui example, you can see how branches allow people to work on code independently, and then once it's complete, merge it back together.
This both allows people to collaborate without stepping on each other's toes, and avoiding committing in-progress code to 
a repository that should be stable.

In the future, I should use branches, but only where appropriate; they can be very useful for reasons mentioned previously, but if used too liberally, can over-complicate the project.
