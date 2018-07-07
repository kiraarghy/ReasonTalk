# ReasonML, the good, the bad, and the ugly.

## Intro:

Hi my name is Kara, I'm a full stack developer at MOO print.

## Section 1: The Journey before the Journey.

### Slide 1

I think I first heard about ReasonML on twitter, in September 2017, I thought it was a new project by Facebook to replace React. So as a React nerd, I tried to find what was available, how to get started, and a bit more about it.

I think at that point the ReasonReact docs were pretty sparse, got nowhere beyond finding out something about static types???? and functional paradigms [something I love!]

But I didn't really try to persue it, and it fell to the back of my mind, unbeknowest to me, growing, fermenting, waiting for its time to strike.

### Slide 2

Fast-forwards 3 months, I'd started to work at MOO, their React stack was really different to anything I had worked on before. Firstly we had a Python backend, something new to me. Secondly, we had all things on top of our base react to make our react, more consistent and safer. Notably, es-lint and flow-types.

If you aren't aware, es-lint is a linter and will throw errors/ warnings if your javascript doesn't follow certain criteria/ structures.

Flow-types, provide static types for javascript at compile time. An example of this would be:

```javascript
type zed = string;

const blah: zed = "bip";
```

This typing, gives you some certainty over the types being passed around in your codebase, and should protect you from run-time issues.

Alternatives would be typescript, and prop-types.

Having worked with this for a couple of weeks however I found these incredibly frustrating, eslint, flow, React, my IDE would all conflict with each other if there were issues don't get me started on the flow runner in our gitlab pipelines.

### Slide 3

I thought there must be a better way. There must be right?

The little nugget of information I had learnt the September before suddenly bore fruit. A voice in my head said, how about you look into Reason again?

So I did!

Now where to get started?

🕵️‍♀️

## Section 2: The beginning.

### Slide 4

Now is a good time to give a good intro to what Reason, Reason React, and bucklesript are?

### Slide 5

So I began with @ur_friend_james' "A First Reason React app for Javascript developers", possibly the first article on how to set up a Reason React project for a noob like myself.

The ugly af part of this was setting up my dev environment. At the time I think I spent about 3 hours just messing around with OPAM (Ocaml package manager), merlin, ocaml, before I realised you could just install reasoncli and use that to start everything!

Fortunately nwo the docs have improved a lot and it's simpler to get started.

### Slide 6

The first thing I came to was the component syntax for Reason React, which is sorta similar but weird coming from a React background.

```javascript
let component = ReasonReact.statelessComponent("App");

let make = (_children) => {
  ...component,
  render: (_self) =>
    <div className="App">
      <h1>{ReasonReact.stringToElement("Reason Projects")}</h1>
    </div>
};
```

```javascript
let component = ReasonReact.statelessComponent("App");

/*WUT??*/ let make = (/*WUT??*/_children) => {
  ...component,
  render: (_self) =>
    <div className="App">
      <h1>{/*WUT??*/ReasonReact.string("Reason Projects")}</h1>
    </div>
};
```

Just talking through this, we have a make which is a function within the ReasonReact module. This make function then returns a record. Oh yeah a record is like an object, but not a JS object, we'll get to that later.

The ...component is the return of the component, allowing us access to the component spec.

The rest of the component is pretty much the same as a normal React component. Except for the 'ReasonReact.string("Reason Projects")' which is just returning an element with the string inside of it.

Pretty simple right?

### Slide 7

But it sucked, there is a really annoying uncanny valley situation where you're like I know what this is, it looks like stuff I worked on every day, but it doesn't work the same way!

You have to convert JSON:

```javascript
let parseRepoJson = (json: Js.Json.t) : repo => {
  full_name: Json.Decode.field("full_name", Json.Decode.string, json),
  stargazers_count:
    Json.Decode.field("stargazers_count", Json.Decode.int, json),
  html_url: Json.Decode.field("html_url", Json.Decode.string, json)
};
```

You have these weird pipe things?

```javascript
|>
```

So I struggled, I followed the instructions as laid out by James for a couple of days, I'd try things but they wouldn't work. I'm not sure due to non understanding configuration, fundamentals or other general things. I didn't understand what I was doing.

I dove into incomplete docs, looked for any resources on the internet to find where I was going wrong!

It felt frustrating, but weirdly refreshing. I was learning something knew, I couldn't find all the answers, I had to work.

### Slide 8

The pay off was massive!

One week later, I had a fairly simple application which pulled repos from the gitHub API. But it felt like I'd completed some sort of marathon.

But more importantly it worked, I'd done it!

### Slide 9

Attack of the MLs!

So my second project, built upon what I'd learnt in the previous project.

How about a pokédex type app?

How about SWAPI - The Star Wars api?

So I bullied one of my best friends James to pair with me, and endure me swearing at my IDE for several hours, I ran into similar issues as above.

And I came up with this:

Pretty nice huh?

### Slide 10

This is where I fell in love.

I fell in love with this amazing thing!

```javascript
/* State declaration */
type state = {
  count: int,
  show: bool,
};

/* Action declaration */
type action =
  | Click
  | Toggle;

/* Component template declaration.
   Needs to be **after** state and action declarations! */
let component = ReasonReact.reducerComponent("Example");

/* greeting and children are props. `children` isn't used, therefore ignored.
   We ignore it by prepending it with an underscore */
let make = (~greeting, _children) => {
  /* spread the other default fields of component here and override a few */
  ...component,

  initialState: () => {count: 0, show: true},

  /* State transitions */
  reducer: (action, state) =>
    switch (action) {
    | Click => ReasonReact.Update({...state, count: state.count + 1})
    | Toggle => ReasonReact.Update({...state, show: ! state.show})
    },

  render: self => {
    let message =
      "You've clicked this " ++ string_of_int(self.state.count) ++ " times(s)";
    <div>
      <button onClick=(_event => self.send(Click))>
        (ReasonReact.string(message))
      </button>
      <button onClick=(_event => self.send(Toggle))>
        (ReasonReact.string("Toggle greeting"))
      </button>
      (
        self.state.show ?
          ReasonReact.string(greeting) : ReasonReact.null
      )
    </div>;
  },
};
```

At the time I was struggling with learning Redux at work. I found the separation of state, action, reducer and middleware. I feel the separation of them into different files didn't help.

Seeing them written out as succinctly as this really helped me understand them, and bring them back into my work. But aslo it's just really freaking nice.

### Slide 11

I then spent the next 2 months not really doing that much Reason, work got pretty intense. But I did continue to immerse myself in the learning, I listened to the great ReasonTown podcast, followed people on Twitter, and even started the new pride in london website in Reason.

Then something amazing happened:

## ReasonConf and the level up!

### Slide 12

Rewind a few months to February, and the ReasonLondon meetup, when I first found out about the Reason Conference, it sounded like my dream place. I had to go, so I did this:

A week before the conference I get a message from one of the conference organisers Nik saying that I got a ticket, I was going!

###Slide 13

Insert picture of me on my way to ReasonConf

## Next level Reason

###Slide 14
