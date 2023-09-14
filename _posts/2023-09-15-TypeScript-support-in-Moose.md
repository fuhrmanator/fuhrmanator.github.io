---
layout: post
comments: true
published: false
title: Analyzing TypeScript projects with Moose
header-img: img/posts/moose-typescript.png
background: '/img/posts/moose-typescript.png'
---

Now it's possible to model and analyze TypeScript projects in [Moose](https://github.com/moosetechnology).

Thanks to excellent collaboration with summer interns, former students who contributed to open-source, and Inria's [EVREF (formerly RMoD)](https://www.inria.fr/fr/evref) group, there is a stable [metamodel](https://github.com/fuhrmanator/FamixTypeScript) and [importer](https://github.com/fuhrmanator/FamixTypeScriptImporter) for TypeScript.
The importer is also an npmjs package, which means you can easily install and run it if you use npm.

In this post, I'll show you how to use these tools to analyse a TypeScript project using Moose.

## Teach Moose about TypeScript

The Moose software needs a metamodel to understand TypeScript programs.
Our project provides this metamodel at <https://github.com/fuhrmanator/FamixTypeScript>. 

The following instructions are for people familiar with Moose and Pharo. If you need help with Pharo images, check out the [Pharo Mooc](https://mooc.pharo.org/). 

- Create and run a **Moose 10 (stable)** image.
- Open a Playground and run this script to load the FamixTypeScript metamodel:
    ```smalltalk
    Metacello new 
        githubUser: 'fuhrmanator' project: 'FamixTypeScript' commitish: 'master' path: 'src';
        baseline: 'FamixTypeScript';
        load
    ``` 

## Create the Moose (Famix) model of a TypeScript project

- Let's clone the project <https://github.com/Chuzzy/Emojiopoly>.
- Next, we'll install `ts2famix` (it's an npm command, so you need to have npm running on your machine): `npm i -g ts2famix`. 
- Let's move to the Emojiopoly clone's location, and create a Famix model of the Emojiopoly project:
   ```bash
   cd path/to/Emojiopoly
   ts2famix -i tsconfig.json -o emojiopoly-model.json
   ```
   This will create the model `emojiopoly-model.json` in the same directory as the Emojiopoly clone.

## Import the model into Moose

- If you're running Windows (or a window manager in Linux?), you can drag the `emojiopoly-model.json` file and drop it on the running window of Moose. You'll see a dialog allowing you to confirm the import.
- If you're not able to drag the file, you have to load it with a script like this in a Playground:
  ```smalltalk
  ```

## Do analyses on the model.

## Creating a Famix model of a TypeScript project

