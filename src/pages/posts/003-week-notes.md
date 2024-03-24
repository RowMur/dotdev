---
layout: ../../layouts/MarkdownLayout.astro
pubDate: 2024-03-24
title: "Week Notes"
subtitle: "Setting up my mac, upgrading my personal site, and building a GQL API"
tags: ["dev", "astro", "graphql"]
---

I'm going to write these "Week notes" posts as a catch all for anything noteworthy but not substantial enough for a post of it's own - a bit of a brain dump. Whether or not I write them weekly is another question and only time will tell...

<h3 class="font-medium text-lg my-4">Setting up my New Mac</h3>

So far, all of my programming experience has taken place on a Windows machine. At home I have a PC and at work I was given a Windows laptop as that was the majority at that time.

<br>

Since then, I've switched teams and am now predominantly surrounded by Mac users - "*scoff* windows" has been a common hearing for me. There have also been times where I've been pairing with a colleague and seen them using some app or tool only to then Google it and find out it is MacOS exclusive - "Join the waitlist for Windows"...

<br>

I now have a Mac, having received it from my Dad - it was an old work laptop of his. It's still fairly new to me however the experience so far has been great. I've been installing stuff and it's all just worked...

<br>

Saying that, I am definitely not yet in a place now to form an opinion on which I prefer. It's still a new shiny thing so I need to get over this honeymoon period. Also, I haven't set anything up yet that was new to me. Of course the first time setting something up is going to be the most difficult. It just so happens that the first time was on Windows.

<br>

The big pluses for Mac so far have been on the apps I can use. I'm now using the [_Arc browser_](https://arc.net/) instead of Chrome, also I've started using [_Warp_](https://www.warp.dev/) for my terminal. Both of these tools are Mac only currently.

<h3 class="font-medium text-lg my-4">Revamping my Personal Site</h3>

My previous personal site was a very simple CRA (create-react-app) project. There was nothing on it, just my name and links to my profiles. I'd been meaning to go back to it for a while but never came round to it and I also wanted to start writing so I made this.

<br>

I'd heard good things of [_Astro_](https://astro.build/) for static sites so thought I'd give it a go. Project init was simple, they've got a nice CLI setup wizard which always helps.

<br>

The folder structure is no different to what I'm used to, working in Next 12 at work with a pages directory for routing. As this is only a simple site I haven't needed to bring in anything like React for interactivity and so I've been solely writing Astro components - it feels no different to writing React components with JSX. Some logic at the top, markup at the bottom.

<br>

Reading more into the docs, I think I've hit the nail on the head with picking Astro for this as it's the exact use case they outline.

<br>

Starting up this project, it also showed me a cool `.vscode` directory feature. Astro's out of the box app has a `.vscode/extensions.json` file which you can use to add recommended extensions to a project. Devs are then prompted by VSCode to install these extensions. This would have been useful in a previous piece of work I did adding a new GQL setup to our project where I added recommended extensions to the contributing docs.

<h3 class="font-medium text-lg my-4">Building a GQL API</h3>

I set a goal of building an instagram clone in my previous post with a focus on learning about the backend using GQL and also making use of AWS. I've made a start on the GQL API this week.

<br>

I've been writing it in Go and using [_gqlgen_](https://github.com/99designs/gqlgen). It makes it a lot simpler than I first expected, you write up a schema, run codegen, and a group of resolver functions will be generated to fill in with the logic.

<br>

One challenge that I've run into so far has been around what I'll call "superset types". There is the standard "user" type that will expose public data about that user, and I wanted to create a "superset type" "currentUser" to add fields such as "apiKey".

<br>

My first implementation used an interface for a base "user" type. I then defined an "otherUser" type which was just the base user type, and a "currentUser" type that had the extra fields - both of these implementing the "user" interface.

<br>

```gql
interface User {
    id ID!
    name String!
    email String!
}

type OtherUser implements User {
    id ID!
    name String!
    email String!
}

type CurrentUser implements User {
    id ID!
    name String!
    email String!
    apiKey String!
}
```

<br>

I wasn't a massive fan of this, I've had to write the same fields multiple times and this doesn't really represent the relationship between these types properly. Examples from GQL docs have "Character" as the interface, and then "Droid" or "Human" as the types implementing.

<br>

Nonetheless, I moved on with this solution until stumbling into another hurdle. I was adding a "followers" field to the "User" type and wanted this field to have it's own resolver. However, this field was now on two types and so I would have had to write the same resolver for both "OtherUser" and "CurrentUser". I was not going to do that as that was even more horrible.

<br>

This brings me up to where it is now, where "User" is a field on "CurrentUser". Going back to "superset type" this is pretty much what I wanted to do, it just didn't occur to me and I saw it in a stack overflow thread. In my head though, I wanted it to be a single flat type which it isn't though. Here's my code now:

<br>

```gql
type User {
  id: ID!
  name: String!
  createdAt: DateTime!
  updatedAt: DateTime!
  following: [Follow!]!
  followers: [Follow!]!
}

type CurrentUser {
  user: User!
  apiKey: String!
}
```
