
![[icon_version_2.png]]

An implementation for the Godot 4.x Engine of the [Valve's Response System](https://developer.valvesoftware.com/wiki/Response_System) used in games like **Left 4 Dead**.  The system is  used in other games of other developers like **Naughty Dog** in **Last Of Us** and by **Campo Santo** developers in **Firewatch**. 

## Background

While our search to find a system to implement a dialogue system for our videogame [Shadow's Keep](https://2therecoop.itch.io/shadows-keep), one of the main challenges was handle and design highly branched dialogues and storytelling that depend of a big number of ingame variables or states.  The common approach is to implement this using conditional statements (if/else statements). However, if you want to create dialogues that give the illusion of being truly reactive, you need to check and modify a huge number of variables and create a large number of possible paths. As the game grows in length, this will undoubtedly lead to the problem known as the "if/else hell" (as shown in the code snippet below), which will severely compromise the scalability of the code.

```
func get_dialog_response(player_input: String) -> String:
    if player_input == "hello":
        return "Hi there, traveler!"
    else:
        if player_input == "how are you?" and guard_health == 100:
            return "I’m just guarding the gate as usual."
        else:
            if player_input == "quest" and is_sick_guard:
	            doSomething()
                return "Yes, I have a task for you. Bring me 5 herbs."
            else:
                if player_input == "reward":
	                doAnotherThing()
                    return "The reward is 100 gold coins."
                else:
                    if player_input == "bye":
                        return "Farewell, stranger."
                    else:
                        # it just keep going!!!!!!!!
```

By chance, thanks to the internet, we found a [GDC talk by Valve](https://www.youtube.com/watch?v=tAbBID3N64A) about the system they implemented and used in several of their video games, which ended up being the solution we had been looking for. After a quick search to see if other developers had implemented a similar system as a plugin for Godot, we found no results, so we decided to implement our own.

## General concepts

**Reaction** can be used to determine which resource to return (or whether to return nothing at all) in a scene, by any component. In our case, the initial need was to return dialogue based on the current state or context of the game. But it can be used to return any resource in Godot. For example: audio, animations, scripts, etc. 

> [!TIP] 
> # Much more than just a dialogue manager!!!!
> One of the common aplication for **Reaction** could be as a dialogue manager but could be 
>  used as a heavy conditional resource return pattern

Generally speaking, it works by using a "data store" or **blackboard** that contains the current state of the game or level (like a dictionary or hash table), which is the single source of truth for **Reaction**. When a component triggers a **Reaction** event, it will check all the **rules** defined for that event. Each rule has different **criteria** defined (the criteria are conditions that must be met in the **blackboard**). The first rule that meets all its conditions will be the one that returns its associated **responses** or resources. 

For more details, see [[How Reaction works]].

