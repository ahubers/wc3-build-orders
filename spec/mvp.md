## Motivation
I want to build spawningtool.com but for wc3 build orders. There is a huge demand for this.
As a sc2 player learning warcraft 3, I was really disturbed to find something like this doesn't exist. After about 
a month of trying to learn the game, I've had to start compiling my own build orders
from 4+ year old videos on youtube. I don't like business or ambition so this is a big step for me,
but I really think a site like this would be wanted and useful.

## Proposal
I want to outline some guiding principles for this project. As a developer who hates side projects, "hustle",
and pet projects, I'm in an odd spot to be building this, or even proposing it. So I want to be clear
what the ambition is.

### Open Source
The site will be open source from the start. Not just for transparency or to be cool because we're
H@ck3rs. It needs to be open source because I'll need help and it will be free and adless and developer time / hosting 
costs fucking money. So it either needs to be monetizable or open source and I'm sure as fuck not monetizing it.

### MINIMALLY VIABLE PRODUCT
I really mean minimal here. Here is the bullet list spec for what I deem minimally viable:
- LIST Page for build orders
- View tracking + upvoe/downvote system. This is necessary for ordering build orders.
- LIST Page Filters: Race, Matchup, Patch.
- LIST Page Sort by: New, Votes, Views.
- DISPLAY Page for a specific build order
- Auth system for users to CREATE, UPDATE, and DELETE build orders.

That's it. No scope creep! Only creeping.

### Minimally viable design set
Looking at our MVP spec, what's the minimum amount of pages we'd need?

- Homepage (LIST)
- DETAIL view
- Auth pages: Login, Register, missing password, all that boring shit that should come with the framework likely.

### Wtf should we call it?
I dunno. Something catchy, like zombocom. We may coordinate with Wc3 Gym clan here and I'd be fine
naming it with their permission as wc3gym.com. 

## Technical implementation proposal
This is supposed to be simple and easy and limited in scope so likewise the tech stack
should follow suit. The simple theme here is what **minimally fits our needs?**

### What back-end framework?
Django? Ruby on Rails? Django is the obvious answer because I have experience in it, it provides
auth right out of the gate, and is quick to prototype.

### What Front-end framework?
Is a front-end framework necessary? My suspicion is no. I think this site can be built
like it's 1995 using something like django or ruby on rails -- i.e, simple server side rendering
with no SPA functionality.

There do exist dom-manipulating frameworks that AREN'T jquery, like vue.js, that we could use
to avoid the evils of Jquery.

### Where/how are we hosting this? Hint: $$ is an issue
My guess is heroku because $$.

### Build pipeline?
CircleCI is actually free for open source public projects and would fit our needs.

### What data store?
Obviously MySQL is easiest but the shape of a build order is easier to model as JSON with something
like Elasticsearch or a NoSQL option. So I would recommend MySQL for auth but a NoSQL store for build orders.
Elasticsearch may be best so that we can order / filter quickly on build orders.

https://bonsai.io/pricing It seems Bonsai offers hosted ES for free.

The most straightforward solution I think is to store the build meta + build steps in MySQL then index
them onto ES. I say that's "straightforward" because it's what I'm familiar with.

### What are our models?
- Auth models, e.g., `users` (supplied by framework)
- `builds` - Would contain build metadata
- `build_steps` - Foreign key to `builds` and contains each steps as rows. 

### Build shape / meta data
Here's a simple JSON prototype with some sample data I've taken from an Orc build.
I've stolen some of the ideas for metadata from https://lotv.spawningtool.com/build/93937/.

You could persistently represent this in the `builds` and `build_steps` table and then index this document into ES.

```JSON
{
    "meta": {
        "votes": {
            "up": 1,
            "down": 1,
            "percent": 50
        },
        "views": 100,
        "user": 12345,
        "patch": "1.2.3",
        "date_created": "ISO date format",
        "date_modified": "ISO date here",
        "race": "OR",
        "matchup": "ORvsX"
    },
    "description": "Optional plaintext description of build here",
    "build": [
        {
            "food": 6,
            "totalFood": 10,
            "description": "Queue Peon, Pull peon from gold, build altar of storms."
        },
        {
            "food": 7,
            "totalFood": 10,
            "description": "Peon pops and builds burrow."
        },
    ]
}
```

## Standardizations of race terminology
I want to point out here that in our models we'll want to standardize the race / matchup terms for clarity and
potential use as ENUMS. Here they are: 

### Races
- `HU` - Human
- `OR` - Orc
- `NE` - Night Elf
- `UD` - Undead

It follows there's 16 + 4 = 20 matchup definitions to follow.

### Matchups

Human:

- `HUvHU` - Human vs Human
- `HUvOR` - Human vs Orc
- `HUvNE` - Human vs Night Elf
- `HUvUD` - Human vs Undead
- `HUvX` -  Human vs any race (e.g, this build can be used generically)

Orc:

- `ORvHU` - Orc vs Human
- `ORvOR` - Orc vs Orc
- `ORvNE` - Orc vs Night Elf
- `ORvUD` - Orc vs Undead
- `ORvX` -  Orc vs any race (e.g, this build can be used generically)

Night Elf:

- `NEvHU` - Night Elf vs Human
- `NEvOR` - Night Elf vs Orc
- `NEvNE` - Night Elf vs Night Elf
- `NEvUD` - Night Elf vs Undead
- `NEvX` -  Night Elf vs any race (e.g, this build can be used generically)

Orc:

- `UDvHU` - Undead vs Human
- `UDvOR` - Undead vs Orc
- `UDvNE` - Undead vs Night Elf
- `UDvUD` - Undead vs Undead
- `UDvX` -  Undead vs any race (e.g, this build can be used generically)
