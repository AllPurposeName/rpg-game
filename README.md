### The Goal
This is a Game Master assistant I'm currently making (9/2015) for the D&D-like
RPG game I am currently playing with my group. The goal of this is to more
easily track and access the different pathways players may choose to take. It
should help alleviate Game Master logistics and in doing so help storytelling
become more the focus of the Game Master's duty.

### The Plan
The game will recursively run a REPL which first asks for a `Premise`. Premises
typically span a few session but can run quite short as well. This length is
usually decided by how many `Encounter`s are involved in each premise, and which
pathways the players choose to travel.

A sample outline could look like such:

```
Players looking for a recently stolen item enter a tavern. They have heard that
a fence with loyalty only to the coin frequents the establishment and they wish
to speak with him.

Premise: The Stolen Hierloom
  Encounter: A Fence Walks Into a Bar...
    Decision Points: Choose how to find the fence amongst the crowd
      A: The ex-town guard character Mosley (the most authoritative of the
      group) approaches the bartender. Leads to D.
      B: The thief Neatsfoote uses her thieves cant to make it known she's in
      possession of stolen goods she's looking to get off her hands ASAP. Leads
      to E.
      C: The foreign warlock Jurai repeatedly corners and charms the
      unsuspecting in hopes of gaining information not only on the fence's
      identity, but also on the local history of The Stolen Heirloom. Leads to
      F.

      D: The bartender knows very well how to handle people like you. Being on
      the darker side of the law himself, he sometimes knows it's valuable to
      make friends with keys to the dungeons. He agrees to finger the fence if
      one of the city gates' portculisses is conveniently open at midnight
      tonight.
        G: Agree to his terms and figure out the rest later. Leads to I.
        H: Politely disagree and ask if there is something of mutual interest to
        your two parties. Leads to J.

      E: Neatsfoote's plan did the trick and almost immediately she finds
      herself in conversation with two shady men. The conversation is dominated
      by the two one-upping each other by offering Neatsfoote a better cut on
      the deal.
        K: Suggest the good is somehow related to the recently missing heirloom
        and experience with noble goods is of upmost priority for your ends.
        Leads to M.
        L: Turn the tables and tell the two fences your stolen goods are
        information, and if the right ears hear about this information, a
        hefty finders fee will be in order. The job is inherently more
        dangerous, but the payout will trump two months of selling trinkets for
        street scum. Leads to N.

      F: The first fool you find gives up the infamous name of the fence with an
      insatiable appetite for coin. You find yourself back with your group in a
      matter of minutes. Before forking up the name, you wonder if some more
      time invested here would pay off.
        O: Decide it's not worth it and give the information to your friends.
        Leads to R.
        P: Continue charming fools and gather a general information about this
        city districts' underbelly. Leads to S.
        Q: Charm your way up the ladder of more prominent members of this seedy
        society. You know the spell's effectiveness will be reduced with each
        rogues increased resistance to giving up the information. Leads to T.

...
...
...

        T: Roll HIGH_CHANCE.to LOW_CHANCE, with each success take a new piece of
        PREMISE_INFORMATION. Decide to quit at any time. There are consequences
        of failure.
          Consequences: If you fail, a brawl instantly breaks out and
          Neatsfoote recognized the secret word for "this one is asking too
          many questions". You take a shot to the chin in the ruckus, but your
          friends get you out of there before it becomes too hairy. Encounter
          finished.

```

### The Guts
#### Characters
Playable characters are going to be simple Ruby classes which all share basic
details from a `Player` superclass. Each player will be custom written since I
don't feel like parsing YAML or something like that.

#### Encounters
Encounters will have specific data such as information, items, and NPCS which
will all behave the same regardless of which playable characters are involved or
which premise is involved.

#### Premises
Premises will wrap the Encounters, supplying the information pertaining
directly to the Game master. This includes: hints to give players, reminders of
past choices, and look-aheads about the available paths. It will also provide
the name generation for quick access.

#### Game
The Game will contain the roll information such as `LOW_CHANCE = "10%"`
`DIE = { "10%" => [{3d6 => 15}, {1d20 => 19}, {2d12 => 19}, {6d4 => 19]`
These settings will be extracted from a YAML file for ease of change and initial
setup.

### The Why
The rules for this game are quite different from a typical RPG. I wanted to
focus more on roleplaying and tough decision making rather than getting caught
up in moves, positions, percentage strategies, and hedonistic strategy.

First of all, each character was contrived with everything the players SHOULDN'T
have control over: The character's racial and 'class' background (rich, poor,
noble, disgraced). The key relationships the character has forged with NPCs and
PCs alike. The relationship the character has to the initial premise.

I've found that giving players too much freedom to customized the above can lead
to characters being completely remote, uninterested in the events around them,
and too invested in the bit of background story they thought was very
compelling. Also limiting the choices from infinity to a dozen really helps
"group cohesion anxiety" that many players face when drafting a character.

Limiting choices isn't only pertinent in the early stages of a campaign, I have
also deemed it necessary for the entire game. If a player can do _anything_,
then _anything_ has to somehow fit into your narrative. Game Masters have
lamented for eons about having a wonderfully put together story which the
characters totally subvert because they want to exert their free will. This
desire to be destructive and careless comes from controlling a surrogate who
isn't held accountable for their actions. After all it's just a game. In
reality, people don't feel like they can do anything. Completely neglecting
what you've agreed to do leads to guilt and a looming burden. Players don't
feel that when they agree to take care of some bandits, but instead get caught
up trying to join the secret guild of assassins.

Yes, the world should have capacity for such things, but no, those are not the
things the people the players are controlling would be interested in. So not
only do I provide the players with realistic characters, but I also provide
those characters with realistic choices.

Another burden in typical RPG format is that there always has to be a battle.
Game Masters are often encouraged to craft battle-less scene's, but it always
feels like a break before the next fight. And players want to fight for good
reason. EXP. Gaining experiences and gathering treasures feels great. It feels
great, but it is a totally contrived system. Relying on fights in this way makes
the game needlessly violent. Sure, sometimes you have to kill some bad guys, but
many problems could be solved _better_ with smarter decisions. A better system
can be made, and it doesn't have to be addictive and self-perpetuating. The
players' reactions to the story should guide change in characters, not having
killed 1000 EXP from various monsters.

The biggest time sapper from our games have historically been two things: A.
Making decisions in battles, and B. Making decisions of what to do next. The
former should be snap decisions which come from a place of "I can do this cool
thing!" rather than "Oh this stronger move is on cooldown, so I'll do the weaker
one. Where is the enemy? Oh that's out of range, what about if I move first..."
Give characters a skill set and have them call upon it. Force uncomfortable
situations which demand hard decisions from the player.
Example:
```
Walking through the decrepid halls of the long abandoned manor you hear voices
ahead. Immediately you identify the whiney voice of the necromancer's right
hand man, Givil. You glance to each other and all recognize the same thing on
the faces you see: justice. You've been after him long enough and now that he's
finally left his guard down, you'll be able to trap him. He won't slip away this
time, this time--

Suddenly from a narrow servant corridor to your right a grey mass descends
upon Jurai. His robes are immediately tangled and then torn. An anonymous vial
shatters on the ground, surely alerting Givil and his guest.

  Jurai: Your inital glimpses of the creature have you thinking it is some type
  of zombie. You're barely holding the thing back as it snaps its teeth inches
  away from your nose.
    Chant a quick slowing spell, sure to give your friends enough time to help
    you. However, the spell will extend down the hallway and Givil will have
    even more time to escape your grasp
    or
    You think you can stave it off by getting more leverage. You move your
    hands from it's shoulders up to it's face. It's more likely to bite you, but
    a hand is much easier to treat than a missing nose.

  Mosley: You took the lead in this hallway. You heard the voices first. You got
  caught up in the excitement of getting one step closer to the necromancer.
  Your torn and have to make a split decision.
    Ease your guilt about letting your guard down and allowing Jurai to be
    attacked by flipping the monster off of him and engaging it yourself
    instead
    or
    ease your guilt about letting the necromancer murder the young prince by
    racing forward to make sure Givil doesn't escape this time.

  Neatsfoote: The clash infront of you knocks you on your ass. Everything seems
  to be happening in slow motion. You see Jurai flailing backwards against
  snapping jaws. Your blood starts to boil just as your stomach lurches.
    Get to your feet, pull out your force wand and try to blast the creature off
    of Jurai
    or
    unsheath your trusted dagger and dive forward onto the creature with your
    full weight and ferocity.
```

A system presented like this allows for some things severely lacking in the
typical D&D format:
* Each player's decision can be secret, causing clashes, solidarity, and
sometimes calamity.
* Each player is guaranteed to have an impact in the situation, rather than
being the person standing in the corner who has missed 7 attacks in a row and
is ready to quit forever.
* Each player is held accountable because they were given clear choices and
have to make the decision which feels the most right for them at the time.
* The situation is tailored to the current inventory, investment, and skill set
  of the players involved. Without specific knowledge that Neatsfoote stole that
  force wand from the raid on the necromancer's safe house, the options for that
  character become generic and "do an attack or look around".

In the regular system, it would come down to "we have to kill these guys ASAP
so we can get to the _real_ fight." The strategy then becomes, "how do we kill
them fast, but also preserve as much power as we can for the next fight". There
is no option for reckless abandon. The urgency revolves around some hit
point/healing system. The scope has been set, it mini-fight time. If Mosley
flees from combat, the Game Master has to punish him for spoiling the narrative.
It's not built for passionate roleplaying.
