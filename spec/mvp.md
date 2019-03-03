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
I dunno. Something catchy, like zombocom.

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

The only issue is ES is not intended to be a persistent store, so we may need to explore other NoSQL options.
Please don't say Mongo. But it might be Mongo. But hopefully not Mongo.

### What are our models?
- Auth models, e.g., `users` (supplied by framework)
- `Builds`

What else?
