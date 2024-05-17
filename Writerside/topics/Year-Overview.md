# Year Conclusion

### What Went Well

Looking back, I believe the biggest thing I learned this year was an understanding of what within game programming I am 
interested in. I found myself most engaged with lower level topics - game engines, programming for platforms, and all the
things that come with that. Writing code that reads in data formats and works a lot with raw memory for my CSGO project 
(an example of which is below) was very interesting to me.

```C++
TArray<uint8> UPAKFile::GetFileDataCopy(const FString& InFilename) const
{
	// To get a file data copy, we first get the file entry, then we use that to find the position of its data in the FileData, and return a copy of that data.
	TArray<uint8> Data;
	FQPAKEntry Entry = GetFileEntry(InFilename);
	if (Entry.Offset == -1)
	{
		Q_LOG_ERROR("File not found in PAK: %s", *InFilename);
		return Data;
	}
	Data.SetNum(Entry.Size);
	FMemory::Memcpy(Data.GetData(), FileData.GetData() + Entry.Offset, Entry.Size);
	return MoveTemp(Data); // Use MoveTemp to avoid copying the data
}
```

With that said, I also enjoyed working with a team to create gameplay that was enjoyable.
It was very fun to present Pirate Survivors at the expo and see people enjoying the game 
(even if multiplayer broke; we should've tested!). In the future, I'd like to playtest more - as well as being good for
making informed design decisions, seeing people enjoy your game can be a massive motivation boost.

### What Went Poorly

Communication within teams was a constant pain point during this year (an issue in _all 4_ GDEV projects).
We would go weeks without a single communication between team members, and we'd find no one did any work over that time. 
This led to us falling behind, and made it hard to recover as people were not on the same page as to what needed to be done. 

Time management was poor. I ended up doing 90% of the work in about 10% of the allocated time. 
This was partially due to a lot of family stuff occurring during the middle of the year, but a lot of the blame lies with me
just failing to plan properly.

Related to this assignment, I believe I need to work on my writing, and specifically my ability to put things succinctly.
After years of being trained to have to hit word counts, I've found that in university I often write _too much_. 
Over the next year, I would like to get better at knowing what is relevant and what isn't.

### What I'll Do Next Time

Now that I further understand what I'm interested in, I'd like to follow this goal further. For example, I have access to
the PS5s, so I'd like to start getting into writing code for them. I think my big project over the summer and throughout 
the next year should be to get the beginnings of a game engine running. If done right, this could potentially also be the 
basis for multiple module's coursework.

I think I have learned that planning is an invaluable process that would help a lot with time management. If I spent more
time reasoning about a problem before jumping into it, I would be able to split it up and give myself small deadlines. 
It would also let me have small milestones that could help maintain motivation.

Finally, I need to communicate more. This mainly applies to group projects - I need to be more proactive about knowing what
people are doing, and suggesting what they could do if they don't know. As well as this, I should communicate to lecturers 
more about things I am struggling with (such as choosing a project).
