# ForwardJS Notes

<!-- TOC depthFrom:2 depthTo:3 withLinks:1 updateOnSave:0 orderedList:0 -->

- [EmbeddedJS: Advances in NodeBots _Kassandra Perch_](#embeddedjs-advances-in-nodebots-kassandra-perch)
- [The Seif Project _Douglas Crockford_](#the-seif-project-douglas-crockford)
- [ES6, ES7 _Javascript Air Podcast_](#es6-es7-javascript-air-podcast)
- [jQuery to Flux to Elm _Richard Feldman @rtfeldman_](#jquery-to-flux-to-elm-richard-feldman-rtfeldman)
- [Building Web Sites that Work Everywhere  _Dr. Doris Chen_](#building-web-sites-that-work-everywhere-dr-doris-chen)
- [Node, npm, and Service Oriented Architecture _Laurie Voss_](#node-npm-and-service-oriented-architecture-laurie-voss)
- [React-Native: Should I Even Bother Learning Swift? _Brian Douglas_](#react-native-should-i-even-bother-learning-swift-brian-douglas)
- [Building Trust Before Code: Unpacking The Weekly Retrospective _Diane Tate, Dan Mosedale_](#building-trust-before-code-unpacking-the-weekly-retrospective-diane-tate-dan-mosedale)

<!-- /TOC -->

---

### EmbeddedJS: Advances in NodeBots _[Kassandra Perch](https://github.com/nodebotanist) [@nodebotanist](https://twitter.com/nodebotanist)_

- Dev evangelist at Auth0
- Wearables!

started with arduino boards
35 platforms supported by **johnny-five**
**avrdude** - old, used to compile code on the arduino
**avrgirl** - can flash arduino boards in node
**j5-chrome** - run johnny-five bots in the browser
**babblebots** - json object to "thin client"

johnny-five contributor covenant, code of conduct on Working Group

**You don't have to choose between people and code.** Encourage and support people and good code will follow.

forwardjs.nodebotani.st

---

### The Seif Project _Douglas Crockford_

Web is _insecure_ and _complex_.

HTTP, DNS, SSL, Certificate Authorities all have their complexities and issues

HTML pretty much requires Templating

DOM "may be the worst API ever imagined" "just awful"

CSS "crappy stylesheets"

JavaScript is a hot mess
Designed and implemented in 10 days

Helper App with transition plan

seif: publickey @ ipaddress / referral

---

### ES6, ES7 _Javascript Air Podcast_

Stage 0 -> strawman, wacky stuff. "wouldn’t it be neat to do X"

---

### jQuery to Flux to Elm _Richard Feldman @rtfeldman_

_(note: pretty Elm-biased)_

**Architecture**:
 - Validating inputs
 - testing logic
 - rendering updates
 - reusing interfaces

1. Event happens, what do we store?
  - jQuery -> app state goes in the DOM
  - Flux -> app state goes in stores
    - inspect the event, update the stores
  - Elm -> App state goes in the model
    - inspect the event, update the model

2. Given selections, determine validity, e.g. `isValid(selectedGenres)`
  - Flux -> pass to `isValid()`
  - Elm -> pass to `isValid()`
  - jQuery -> do query to get app state from DOM e.g. `getGenres()`, **then** call `isValid()`

3. Testing

  We want test coverage - what does that take?

  - Elm -> call `isValid()`, check the result
  - Flux -> call `isValid()`, check the result
  - jQuery -> call `isValid()`, check the result, but that isn't complete test coverage
     - we still need to test the query function that gets app state from the DOM
     - changing styles (not even related to business logic) affects our testing and can break things

   jQuery started off with an advantage (less work, same results), now we have to do more work to validate and more work to test

4. Render validations

 Who alters the DOM?

 * jQuery -> query the DOM, find the notes and mutate the nodes ourselves
 * Flux -> we have to start talking about React because it's the rendering system it was designed to be used with
    - Component describes the desired DOM in React
 * Elm -> has something similar, a view function. Model -> Desired DOM

#### What happens when you found a bug?

How did the DOM end up in this state? Reproducibility is incredibly important. Want to go back and look at application states and see how they changed over time. Being able to take appliaciton state and generate a DOM and see what it looks like

Flux, Elm -> App state = single source of truth

#### Reusing interfaces

  EX: select subgenres, an accordion instead of a dropdown, but we want them to be reusable
  User clicks to expand accordion -> what state changes?

 - jQuery -> change the DOM, toggle an attribute (app state lives in the DOM)

 - Flux -> Change a store, have to use React + Flux.
    - React has its own way of doing this. React uses drop-in components
    - components manage their own states
    - Who wins when we can do either? **change store** (Flux) or **component** (React) state?
    - Sidestepping flux is the path suggested by libraries => mutate component states
    - What does that mean for reusability?
    - **Querying is back in** (a la jQuery), we have to ask each of our components and ask for their state. gathering scattered pieces of state to assemble the whole picture

 - Elm -> If we need it to render the UI, it goes in the Model. Accordion expansion state goes in the Model
    - Reproducibility still works and scales, this is how elm can have a time traveling debugger that just works. App state is all in one place, in the model

###### _Can we get reproducibility with React + Flux?_
Yes, if we use 100% stateless components, including all third-party components

  - jQuery -> deferred effort, least maintainable
  - Flux -> simpler system, more maintainable, starts to get more complex
  - Elm -> simplest, maintainable

(company **noredink**, uses Elm extensibly in production)

---

### Building Web Sites that Work Everywhere  _Dr. Doris Chen_

Edge is designed to be more compatible with current major browsers.

#### Testing tools

- Site Scan _(Free)_
  - Reports back on comon coding problems
- Browser screenshots _(Free)_
- Windows VMS
- Remote _(Free)_
  - small client app to test Edge behavior from the cloud
- BrowserStack _(Paid service)_

#### Feature detection

**Modernizr**

---

### Node, npm, and Service Oriented Architecture _Laurie Voss_

 ###### SOAs are a distributed system:
   - concurrency
   - no global clock
   - independent component - failures don't propagate to other parts of the system

 ###### What about microservices?
 Not a huge difference (one takes lots longer)

#### SOA Advantages

- **technical**
  - resource efficiency. 4x large app vs 8x small app (very oversimplified, but this works regradless of your constrained resource)
  - cost efficiency
    - **specialization** - you can buy hardware that is specialized to your needs
    - **scalability** - you can scale only the parts that you needs
    - **tunability** - you can tune your redundancy, e.g. you will want redundancy for front page of app but maybe not for marketing pages
  - robustness
    - in a monolith one bad deploy takes down the whole app
    - in a SOA only the one thing you were messing with would go down
    - simple math - 100 boxes, 1% chance of failure, changes of being _completely_ down is 0.01*100. chance of being _slightly_ down is 0.01*100 = 100%
    - you need to be really good at replacing hardware because it's a weekly thing vs a every few months thing
    - prior to 2013 npm would be down for hours at a time. as of now it's more like 10 minutes per year
  - debuggability
    - one box only ever has one service = one to one relationship to the service that's broken to the machine that needs fixing
    - only a certain amount of unique code to each service, so more simple to debug
- **organizational**
  - Conway's Law in reverse (software reflects the structure of the organization that built it).
  - simpler systems are simpler. a service is a small isolated domain of understanding, much easier to fit the understanding into the human brain
  - smaller systems allow for smaller teams - communication within smaller teams is drastically better. Eg. even a 5 person team can have huge communication issues. impossible to get say a 50 person team on the same page.
  - smaller teams are better teams. big teams are hard, so don't have any. better sense of ownership because you can fix the whole system into their brain - better tests, better debugging, etc

#### Why node is great at SOA

1. _shit i missed it_
2. Small modules make for efficient microservices
3. node and npm make shared logic easier
    - node built with a package manager in mind
    - literally impossible for dependencies to fight for eachother in node

#### product plug:

- ❗️ share code within your company using [npm On-Site](https://www.npmjs.com/onsite), or [npm Organizations](https://docs.npmjs.com/orgs/what-are-orgs)

#### Best practices for SOA in node:

- **application level**:
    - isolation
- **service level**:
    - consistency
        - logging
        - error handling
          - _do not recover._ if we knew how to recover, the crash wouldn't have happened.
          - crash on error
          - restart on crash
          - monitor restarts
    - shared logic
      - the less unique code, the faster the fix
      - logging, errors, etc should be a module
      - the smaller you can keep the volume of unique code in your service, the easier it will be to debug. 200 lines for a service is _super_ easy to understand and debug
- **architectural**:
  - async ops will save your life
  - fail safely and softly
    - expect flaky networks
      - detect a network hang. amazon will have a hiccup, things will hang for 60 seconds
    - degrade gracefully
    - recover automatically
    - if authentication service is down, its failure should show public content
  - race conditions
    - there is no magic bullet
    - `conditions. So race many`
- **operational**:
    - configuration
      - one source of truth
      - configure at deploy, not at startup or restart
      - service should only change config when it is _explicitly expected to_
- **deployment**:
  - deployment has become a constant thing
  - automate **everything**
  - use "canary" servers - deploying code to production but only to say 2% of traffic.
    - people do really really weird things in production
  - handle cold starts
    - can the service deploy if it's down?
    - will creep up on you slowly
- **metrics**:
    - capture every request: source, frequency, size
- **monitoring & alerting**:
  - real time dashboards - feels "enterprisey" but is the fastest form of debugging
  - alert on out-of-range metrics

#### What is npm?
1. CLI
2. registry
3. www
4. servers

Really one big thing -> gigantic SOA

- **The CLI**
    - largest consumer of the registry
- **the Registry**
    - 99% of requests dont talk to the auth service (do this if you can get away with it!)
    - _{weee diagram}_
    - User APL, license API, binary/metadata stores, etc
    - **followers** (new thing)
      - using cache DB as an event queue
      - credentials follower: seeing if you accidentally left your credentials in your package
      - tarball follower: if you publish a package, it needs to go to all the servers worldwide. lets you publish to just one locale and copies it to one of the other ones (creates a race condition, has some followup logic in the CDN to account for  this)
  - **www**
    - registry API follower
    - elastisearch follower
  - **npm On-Site**
    - literally a copy of the registry architecture but crammed down into wherever you want it to be
    - can be super confident about the scaling abilities of npm offsite because your companies registry isn't going to be as big as all the npm registries in the world ;)

---

### React-Native: Should I Even Bother Learning Swift? _Brian Douglas_

  Uh shit I got distracted :(

  Repos should be at [@bdougie](https://github.com/bdougie)

---

### Building Trust Before Code: Unpacking The Weekly Retrospective _Diane Tate, Dan Mosedale [@dmose](https://twitter.com/dmose)_

> If you are familiar with agile practices, you probably know about team retrospectives. And, as with software development in general, your results may vary. But why exactly are they used? And what approaches can we take to make them more useful for software teams?
>
> Diane Tate, Program Manager at Mozilla, will interview Dan Mosedale, one of the co-founders of the Mozilla project and this simple process that can significantly impact your engineering team's effectiveness.
>
> Dan Mosedale is a co-founder of the Mozilla project and is currently writing code for Hello, a web-sharing Firefox feature. Other Mozilla work includes standing up WebRTC in Firefox Android, implementing HTML5 web-based protocol handlers in Firefox desktop, and working on the Gladius HTML 3D gaming framework.


- **What did we do well?**
  - Give ourselves credit. We tend to focus on all the things that are in front of us, but giving ourselves credit really empowers us intsead of getting too overwhelmed by "oh look at all the shit to do"
- **What have we learned?**
  - Act of talking about things makes new things known to other members of the team.
  - More context for what's going on in the project for everyone on the team.
- **What could we have done better?**
  - Almost all the time people give completely undefined points that we don't know how to do better. It's something that **puzzles us**.
- What puzzles us?
  - We're allowed to just leave shit on the table.
    - This prevents punishing the messenger - when someone brings up an issue / puzzle and then is forced to own it.
  - What are some small experiments we could try to make it a little bit better? Having a retrospective every week means you have the chance to try things without just saying "this doesn’t work, we're doing this now and if it doesn’t work well then shit"

At the beginning beginning of a retrospective reviewing the experiments that are in flight. Every week reviewing the list of stuff, declare some things a success, some times you don't know, sometimes you know it's not working - try to think of something else or move on.

Rating 1-10 how you feel along the sprint's timeline, gives you something to look into on what that problem was or what happened around then.

###### _Having 5 in-flight process improvements at a given time:_
  - all one long scrolling google doc with at template at the bottom to copy-paste. at the top a list of successful experiments which is pretty motivating.

###### _Building trust so that people are comfortable talking during retrospectives:_
  - Being willing to admit that we're puzzled
  - Call out people and thank them for things that they did well
  - Building trust means people don't wander around with retrospective pain

###### _Timing & relatively big questions - how long should a retrospective last?_
  - At the beginning of the meeting is 5 minutes where everyone types in silence the answers to these questions. Then we go down the list and people talk us through the things as we hit them. 45 minutes works well when things are pretty stable. If things are changing a lot things tend to go a lot longer.

###### _How to determine who takes ownership of action items when tweaking process_
  - They dont have a scrum master, its sort of an organic mix so people will take the things that make sense
  - Sometimes no one ones to take something - either say a manager will ask someone to do it or we let it die. It's either not important enough to do or it will come back.

**Retrospective is a trojan horse to a better process**, framing it in terms of small experiments.

Sometimes the nature of the problem changes based on the fact that the discussion was had.

**If it's important, it will come back.**

---

### Closing Plenary: The Economy Of Keystrokes [_Kyle Simpson_](https://github.com/getify) [@getify](https://twitter.com/getify)

#### Keystrokes tradeoffs

- `=>` saves characters by stripping `function() {}` keystrokes
- Readability?
  - Code readability has been studied formally - they had an automated, objective metric to measure readability
  - Rated by a developer depends on their set of code patterns - not that they don't understand the language or syntax. **Familiarity** is a really strong component to the notion of readability.
- "**Tribal knowledge**" that is required to easily understand code as a developer.

**Infinite** number of programs that can solve any particular problem.

JavaScript engine will take our code as a _suggestion_ and comes up with a much more efficient way. We have to separate out the details that we're worrying about like `i++` vs whatever, know that the JS engine is just going to figure that out on its own.

##### If our code is just a suggestion to the JS engine, then what is the purpose of code?
First and foremost, code is a method of communication not just to the computer but _to our teammates and our future selves._

> The code could *easily* have been done with just a single and understandable conditional, and the compiler would actually have generated better code, and the could would look better and more understandable.
>

> I want to make it clear to *everybody* that code like this is completely unacceptable.

> -- <cite>[Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds)</cite>

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
>
> -- <cite>[Martin Fowler](https://twitter.com/martinfowler)</cite>

##### reader > writer
The keystrokes that we're spending ought to be spent with the **reader** in mind, _not_ the **writer**.

##### [simplicity matters](https://www.youtube.com/watch?v=rI8tNMsozo0)
Notion that we go after _ease_ and call it _simplicity_ (being the opposite of complexity, being things being bought tightly together). When we go after easiness first, we oftentimes end up with complexity. When value simplicity first, we often end up with opportunities for easiness.

###### `x + y * z`
You are requiring your **reader** to have the tribal knowledge of operator precedence.

###### `x + (y * z)`
These two keystrokes are _so important_ to someone without that tribal knowledge

###### `x = x || 42`
Super clever, but when teaching brand new developers they struggle to understand why this works the way it does. They struggle with where other language that would return something other than a boolean value.

###### `? : vs if-else`
Goes off the rails when you start chaining them together. You've invited the reader of your code to a special set of tribal knowledge called **accociativity** (ternary operators being right associative vs other operators left associative).

Does this mean dumb down your code? Not necessarily. We want to have a culture of learning, where we have code reviews centered around opportunities to write better code. Say a junior dev submits something and a senior devs takes the opportunity to teach them something about the operator and show a better way of doing it.

Biggest narrative to take away from ES6 is that it takes us away from imperative and moves us to a declarative model.

Telling it how to do something vs telling it "this is what i want to have happen, you figure it out"

#### Abstractions?
Absolutely abstractions are good, but it's bad when we hide the abstractions.
Carps 3 laws - any sufficiently advanced tech is indistinguishable from magic.
repurpose as any sufficiently **unlearned** tech is indistinguishable from magic.

Good programmers don't try to hide stuff, they intentionally move things behind abstractions and they know how that code works

#### Code comments

Problem is we're often trying to answer the "what" question - we should be answering the "why" question.

`i++ // add one to i`
vs.
`i++ // increment i because...`

#### Make every keystroke and every keystroke saved count for **REAL VALUE**

Not saying never use the shortened form or always use the longer form. Make code more _teachable_.
