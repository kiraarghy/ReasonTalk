# ReasonML, the good, the bad, and the ugly.

## Intro:

Hi my name is Kara, I'm a developer at MOO print.

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

Maybe now is a good time to give a good intro to what Reason, Reason React, and bucklesript are?


### Slide 5 

So I began with @ur_friend_james' "A First Reason React app for Javascript developers", possibly the first article on how to set up a Reason React project for a noob like myself. 

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

Now lets talk about the good bits! 

### Slide 6 

A love affair with reducer components! 

