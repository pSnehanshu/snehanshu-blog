---
layout: post
title:  "Svelte, the framework without any framework - A basic tutorial"
categories: [tech, web]
tags: [svelte, spa, frontend, javascript, react, vue]
img: svelte.png
---

# What is Svelte?

[Svelte](https://svelte.dev) frontend web framework like [React](https://reactjs.org/)
and [Vue](https://vuejs.org/) that allows developers to build complex web apps such as
a Single Page Application.

# How is it different?

Well, with frontend frameworks like React, your final application will have the framework's code and code written by you. This is because these frameworks do most of their job in the
browser, so even a simple application can become huge. Apart from increasing download time, it will also increase the time the browser takes to render the page because it has to first execute the framework's code. In mobile devices, it can easily degrade the user experience.

But with Svelte, most of the framework's jobs are taken care of during the build step,
and so it outputs pure vanilla JavaScript. Apart from a few utility functions, the final
application has very little Svelte code. Due to this, the code the browser has to download
and execute before rendering the page is significantly less, resulting in a better user experience.

In short, Svelte is a compiler that generates JavaScript, while React and Vue are JavaScript code themselves.

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
![Svelte initial screen](/assets/post-images/svelte-tutorial/initial-installation.png)

Now that you have Svelte running, let's see how to use it.

# How to use it?

Svelte is based upon the components system. Modern web applications are complex, so dividing them into components
makes sense. This way a developer can focus on a single functionality at a time which increases developer efficiency
and it also makes the code reusable. Imagine you build a comment box component, now you can use it over and over
in different places of your application without copy-pasting the code.

Svelte components are files with `.svelte` extension. It is just a simple HTML file with a `<script>` section, a
`<style>` section, and the remaining is just plain HTML.

```html
<script>
let name = 'Rohit';
</script>

<style>
p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
}
</style>

<p>Hello {name}</p>
```

Result

![Example component result](/assets/post-images/svelte-tutorial/example-component.png)

Now let's modify the data `name` using a function.

```html
<script>
let name = 'Rohit';

function changeName() {
  name = 'Ashish';
}
</script>

<style>
p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
}
</style>

<p>Hello {name}</p>

<button on:click={changeName}>Change name</button>
```

![Name changes on clicking the button](/assets/post-images/svelte-tutorial/Screencast-2020-03-11-164019.gif)

Let's instead of changing to "Ashish" every time, let's take that as an input from the user using an input text box.
First, we will need to define another variable `inputName` to keep the temporary text typed into the text box.
Then on clicking the button, we will just do `name = inputName`.

```html
<script>
let name = 'Rohit';
let inputName = '';

function changeName() {
  name = inputName;
}
</script>

<style>
p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
}
</style>

<p>Hello {name}</p>

<input type="text" bind:value="{inputName}">

<button on:click={changeName}>Change name</button>
```

![Change name from input](/assets/post-images/svelte-tutorial/Screencast-2020-03-11-165008(1).gif)

Instead of changing the name on clicking the button, we can directly change the name from the
text box itself. Instead of binding it to `inputName`, directly bind it to `name`. Now we neither
need the `inputName` variable, the `changeName` function, nor the button.


```html
<script>
let name = 'Rohit';
</script>

<style>
p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
}
</style>

<p>Hello {name}</p>

<input type="text" bind:value="{name}">
```

The UI updates as you type on the text box.

![Update on type](/assets/post-images/svelte-tutorial/Screencast-2020-03-11-170010.gif)

Now, let's look at the `if..else`

We have the following data, it is a list of websites with their founder's name.

```json
{
  "facebook.com": "Mark Zuckerberg",
  "google.com": "Larry Page",
  "paypal.com": "Peter Thiel",
  "snehanshu.tech": "Snehanshu Phukon",
  "ma.tt": "Matt Mullenweg",
  "paytm.com": "Vijay Shekhar Sharma"
}
```

We will take a website name as input from a text box, and then show the founder's name.
If the website doesn't exist in our list, we will show that we don't know the founder.


```html
<script>
const data = {
  "facebook.com": "Mark Zuckerberg",
  "google.com": "Larry Page",
  "paypal.com": "Peter Thiel",
  "snehanshu.tech": "Snehanshu Phukon",
  "ma.tt": "Matt Mullenweg",
  "paytm.com": "Vijay Shekhar Sharma"
};

let website = '';
</script>

<style>
p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
}
</style>

{#if website.length < 1}
	<p>Please type a website name</p>
{:else if data[website]}
	<p>Founder of <u>{website}</u> is <b>{data[website]}</b></p>
{:else}
	<p>We don't know about the founder of <u>{website}</u></p>
{/if}

<input type="text" bind:value="{website}" placeholder="Type website name">
```

![if-else demonstration](/assets/post-images/svelte-tutorial/Screencast-2020-03-11-172020.gif)

Let's take a look at loops, we have a list of websites like this

```json
[
  "facebook.com",
  "google.com",
  "paypal.com",
  "snehanshu.tech",
  "ma.tt",
  "paytm.com"
]
```

We are going to display this list as an [unordered list](https://www.w3schools.com/html/html_lists.asp).

```html
<script>
const websites = [
  "facebook.com",
  "google.com",
  "paypal.com",
  "snehanshu.tech",
  "ma.tt",
  "paytm.com"
];

let website = '';
</script>

<h2>List of Websites</h2>
<ul>
{#each websites as website}
	<li>{website}</li>
{/each}
</ul>
```
![](/assets/post-images/svelte-tutorial/Screenshot from 2020-03-11 17-31-06.png)

Now that we know basic Svelte, I would insist you head over to
the [official tutorial](https://svelte.dev/tutorial) to learn the rest of Svelte.

Thanks for your time.
