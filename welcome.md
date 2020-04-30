## Why this playground?

Creating your first bot (or AI) on CodinGame can be daunting, even scary for some. I've had a lot of great developers tell me they didn't know anything about AI programming and that they were not sure they were good enough to start.

Let me tell you all something.

If you know the basics of programming (variables, loops, conditions, etc..), you can write bots. It's not a part of programming reserved for the elites or something. You can write bots.

Let me guide you!

## What's bot programming on CodinGame?

Bot programming on CodinGame takes the form of multiplayer games where you write a program that is able to play the game. Once submitted into the arena, your bot is matched against hundreds of other bots and you get a rank accordingly.

## Let's get started!

Disclaimer: this tutorial aims at showing how easy and straightforward it can be to get started into the game. 

Let's see how fast we can get to the Bronze league of Ghost in The Cell.

1. Understand the game

Don't hesitate to play the default code to see what the game is about visually. Then you can quickly scan the statements for the main information. You don't need to worry about details from now.

After a few minutes, we quickly understand that:

- each player controls a factory that automatically creates cyborgs
- each player can move cyborgs between factories to extend their territory

And this is enough for now.

2. The dumbest bot

Let's get into the code already and try to implement the first easiest strategy that comes to mind:

"Find a factory that I control and send any cyborg in it to a random factory"

I don't even need a class for this! I just need to get some data from the input, so I'm quickly checking the protocol (below the statement). I need:
- the list of factories (list of entities)
	and for each
	- its id (`entityId`)
	- its owner (`arg1`)
	- and the number of cyborgs it produces (`arg2`)

After playing my first code against the boss, I noticed 2 bugs:

- I mistook a cyborg given in input for a factory because I didn't check the variable `entityType`
- I needed my bot to WAIT if I don't own a factory (happens when all my cyborgs left are on route to their destination)

I fixed that quickly, and :tada:

```java
int entityCount = in.nextInt(); // the number of entities (e.g. factories and troops)
int myFactoryId = -1;
int enemyFactoryId = -1;
int troopsNb = 0;
for (int i = 0; i < entityCount; i++) {
    int entityId = in.nextInt();
    String entityType = in.next();
    int arg1 = in.nextInt();
    int arg2 = in.nextInt();
    int arg3 = in.nextInt();
    int arg4 = in.nextInt();
    int arg5 = in.nextInt();
    if (entityType.equals("FACTORY")) {
        if (arg1 == 1) {
            myFactoryId = entityId;
            troopsNb = arg2;
        }
        else {
            enemyFactoryId = entityId;
        }
    }
}

if (myFactoryId != -1 && enemyFactoryId != -1) {
    System.out.println("MOVE "+myFactoryId+" "+enemyFactoryId+" "+troopsNb);
}
else {
    System.out.println("WAIT");
}
```

I'm sure you agree it's not . The code is not pretty at all but it's normal. 

This bot somehow beat the boss in the IDE! For the Wood leagues, it's usually a good sign to submit your code so I submitted this dumb bot. It got me in the top 10 of the Wood 3 league.

Of course this bot can be improved. Let's see how.

3. Iterate with simple improvements

From there, we can start to list all the simple ideas to improve the bot to make it a bit less dumb:

1. Ignore factories that don't produce any cyborgs (the cyborgs I'll use to take this factory will be lost)
2. Use the factory where I have the most cyborgs to send troops (to avoid sending too few every turn while there are dozens sleeping in an another factory)
3. Focus on the closest factories first (so I take new factories and start producing as soon as possible)

These seem simple enough. Idea 1 is a if condition. Idea 2 is a search for a max. Idea 3 may a bit more complex; I'll implement the first 2 and keep this one in store.

Done in a few minutes. Let's submit!

:boom: I'm promoted to Wood 2! And already ranking in the top 10 of Wood 2 league! That was easy, wasn't it?

4. Discover new rules

New Wood league usually means more rules. Here, I'm told I can now output several MOVE commands during one turn.

It's giving me a new simple idea:

4. For each of my factories, send troops every turns.

Now I'll start writing classes to store the input. Some players like to store all input; I prefer to pick only the data I need, and add new variables only if I need it. I still need to read all input though, else the system will think my code timed out.

So, I'm implementing idea #4 before #3 because I'm lazy and I don't want to store the distance between factories yet...
10 minutes later, I've got a list of all factories and a for loop in which for each of my factory, I send all its troups to the same random enemy factory. Submit and... no improvement. Still in the top 10 of Wood 2. Let's look into idea #3 then.


Sometimes, it can help to consider the game as if it was a board game. "With this board state, what is the best action I can do?"

## Classic mistakes

- Not reading all input (as provided in the default code) will break your code.
- Make sure to output one command per turn (in this game, one command can be made of multiple moves if they're separated by ";")

# Advanced usage

If you want a more complex example (external libraries, viewers...), use the [Advanced Java template](https://tech.io/select-repo/385)
