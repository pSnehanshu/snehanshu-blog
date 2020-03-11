---
layout: post
title:  "Svelte, the framework without any framework"
categories: [tech, web]
tags: [svelte, spa, frontend, javascript, react, vue]
---

# What is Svelte?

[Svelte](https://svelte.dev) frontend web framework like [React](https://reactjs.org/)
and [Vue](https://vuejs.org/) that allows developers to build complex web apps such as
a Single Page Application.

# How is it different?

Well, with frontend frameworks like React, your final application will have the framework's code and code written by you. This is because these frameworks do most of their job in the
browser, so even a simple application can become huge in size. Apart from increasing download time, it will also increase the time the browser takes to render the page because it has to first execute the framework's code. In mobile devices, it can easily degrade user experience.

But with Svelte, most of the framework's jobs are taken care of during the build step,
and so it outputs pure vanilla JavaScript. Apart from a few utility functions, the final
application has very little Svelte code. Due to this, the code the browser has to download
and execute before rendering the page is significantly less, resulting in better user experience.

# How to install it?

* First you will need Node.js.
    * Check if you already have Node.js installed by running this command
```bash
$ node --version
```
    * If error occurs, find appropriate instructions for installing Node.js based on your OS [here](https://nodejs.org/en/download/).
* Next run the following command to get the Svelte project template
```bash
$ npx degit sveltejs/template my-svelte-project
```
* Then switch to the project directory
```bash
$ cd my-svelte-project
```
* Install all the dependencies from NPM
```bash
$ npm install
```
* Test the project in your browser
```bash
$ npm run dev
```
* Open [http://localhost:5000](http://localhost:5000) in your browser and you should see the following
![Svelte initial screen](/assets/svelte-tutorial/initial-installation.png)

Now that you have Svelte running, let's see how to use it.