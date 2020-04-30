**Disclaimer**: this tutorial aims at showing how easy and straightforward it can be to get started into a bot programming game. This is not the cleanest way to do it, that's for sure.

Let's see how fast we can get to the Bronze league of _Ghost in The Cell_.

### Understand the game

Don't hesitate to play the default code to see what the game is about visually. Then you can quickly scan the statement for the main pieces of information. You don't need to worry about details right now.

After two or three minutes of reading, we quickly understand that:

- you control a factory that automatically creates cyborgs
- you can move cyborgs between factories to extend your territory
- you win if you have the most cyborgs, or if all opponents cyborgs are destroyed

And this is enough for now.

### The dumbest bot

Let's get into the code already and try to implement the first simplest strategy that comes to mind:

"Find a factory that I control and send any cyborg in it to a random factory"

I don't even need a class for this! I just need to get some data from the input. For this, I quickly check out the game protocol (below the statement). I need:
- the list of factories (list of `entities`)
	and for each one
	- its id (`entityId`)
	- its owner (`arg1`)
	- and the number of cyborgs it contains (`arg2`)

gif typing

A few if conditions later, I play my first code against the boss and notice 2 bugs:

- I mistook a cyborg given in input for a factory because I didn't check the variable `entityType`
- I need my bot to WAIT if I don't own a factory (happens when all my cyborgs left are on route to their destination). Probably not critical to fix right now, but it's also cool to ensure I output valid commands only.

Let me fix that quickly... :tada:

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

I'm sure you agree it's not rocket science. The code is not pretty at all but it's normal and for now, we don't care. Bear with me, we'll improve it. 

This bot somehow beats the boss in the IDE! For the Wood leagues, it's usually a good sign that you should submit your code right away because you have a good chance to get promoted. So let's submit this dumbest bot. It gets me in the top 10 of the Wood 3 league. Not bad for the dumbest bot ever.

Let's see improve it.

### Iterate with simple improvements

From there, we can start to list all the simple ideas we have to improve the bot and make it a bit less dumb. 

1. Ignore factories that don't produce any cyborgs
_They are useless (for now), and the cyborgs I'll use to take these factories (by fighting enemy or neutral cyborgs) will be lost._
2. Use the factory where I have the most cyborgs to send troops from
_We want to avoid sending too few troops (or even 0) every turn while there are dozens of cyborgs sleeping in an another factory._
3. Focus on the closest factories first
_We want to take new factories and start producing new cyborgs as soon as possible._

These seem simple enough. Idea #1 is one _if_ condition. Idea #2 is a search for a max. Idea #3 may a bit more complex for now so I'll implement the first two and keep this one in store.

It took me longer to write this than to code it. Let's submit!

:boom: I'm promoted to Wood 2! And already ranking in the top 10 of the Wood 2 league! That was easy, wasn't it?

### Discover new rules

Every new Wood league usually comes with a new rule to learn. Here, I'm told I can now output several commands per turn.

It's giving me a new simple idea that I add to my list:

4. For each of my factories, send troops every turn.

So, I'm implementing idea #4 before #3 because I'm lazy and I don't want to store the distance between factories yet...

Now I'll start writing classes to store the input.

_Some players like to store all input directly; I prefer to pick only the data I need, and add new variables only if I need them._

10 minutes later, I've got a list of all factories and a for loop in which for each of my factory, I send all its troups to the same random enemy factory. Submit and... no improvement. This bot is still reaching the top 10 of Wood 2 though.

Let's look into idea #3 then. At least, my code is a bit better cleaner and I know how to output multiple commands in one turn.

So, until now, I only focused on some entities. Now, I need to focus on the map, here, a graph. Let's store the distances between factories during the first turn (since it's given before the while loop). I use a Map of Maps in Java.

I'm adding a bit of logic before my loop to output my commands to get the closest enemy (or neutral) factory which produces cyborgs, and this for each of my factory.

It's simply done with 2 for-loops.

After a few minutes of coding, I submit again! :confetti: Wood 1, here I come. And it seems this code is good enough to even be promoted to the Bronze league. No worries though, I can quickly scan the statement to check for the new rules.

### What's next?

My advice is to continue iterating. Just continue implementing the simplest ideas you can find to improve your bot's decision making.

Sometimes, it can help to consider the game as if it was a board game: "With this board state, what is the best action I can do?"

With this strategy, you should be able to reach Silver in most of bot programming games. 