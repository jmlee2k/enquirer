<h1 align="center">Enquirer</h1>

<p align="center">
<a href="https://npmjs.org/package/enquirer">
<img src="https://img.shields.io/npm/v/enquirer.svg" alt="version">
</a>
<a href="https://travis-ci.org/enquirer/enquirer">
<img src="https://img.shields.io/travis/enquirer/enquirer.svg" alt="travis">
</a>
<a href="https://npmjs.org/package/enquirer">
<img src="https://img.shields.io/npm/dm/enquirer.svg" alt="downloads">
</a>
</p>

<br>
<br>

<p align="center">
<b>Stylish CLI prompts that are user-friendly, intuitive and easy to create.</b></br>
<sub>>_ Prompts should be more like conversations than inquisitions▌<sub>
</p>

<br>
<br>

Enquirer is fast, easy to use, and lightweight enough for small projects, while also being powerful and customizable enough for the most advanced use cases.

* **Fast** - [Loads in ~4ms](#performance) (that's about _3-4 times faster than a [single frame of a HD movie](http://www.endmemo.com/sconvert/framespersecondframespermillisecond.php) at 60fps_)
* **Lightweight** - Only [one dependency](https://github.com/doowb/ansi-colors).
* **Easy to use** - Uses promises and async/await to make prompts easy to create and use.
* **Intuitive** - Navigating around input and choices is a breeze. Advanced keypress combos are available to simplify usage. You can even [record and playback keypresses](recipes/play.js) to aid with tutorials and videos.
* **Multilingual** - Easily add support for multiple languages.
* **Extensible** - Prompts are easy to create and extend.
* **Flexible** - All prompts can be used standalone or chained together.
* **Pluggable** - Add advanced features to Enquirer with plugins.
* **Stylish** - Easily override styles and symbols for any part of the prompt.
* **Validation** - Optionally validate user input with any prompt.
* **Well tested** - All prompts are well-tested, and tests are easy to create without having to use brittle, hacky solutions to spy on prompts or "inject" values.

<br>
<hr>
<br>

## ❯ Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save enquirer@beta
```

_(Requires Node.js 8.6 or higher. Please let us know if you need support for an earlier version by creating an [issue](../../issues/new).)_

<br>
<hr>
<br>

## ❯ Getting started

Get started with Enquirer, the most powerful and easy-to-use Node.js library for creating interactive CLI prompts.

* [Usage](#-usage)
* [API](#-api)
* [Options](#-options)
* [Performance](#-performance)
* [Credit](#-credit)

<br>
<hr>
<br>

## ❯ Usage

### Single prompt

Pass a [question object](#prompt-options) to run a single prompt.

```js
const { prompt } = require('enquirer');

const response = await prompt({
  type: 'input',
  name: 'username',
  message: 'What is your username?' 
});

console.log(response); 
//=> { username: 'jonschlinkert' }
```

_(Examples with `await` need to be run inside an `async` function)_

### Multiple prompts

Pass an array of ["question" objects](#prompt-options) to run a series of prompts.

```js
const response = await prompt([
  {
    type: 'input',
    name: 'name',
    message: 'What is your name?' 
  },
  {
    type: 'input',
    name: 'username',
    message: 'What is your username?' 
  }
]);

console.log(response);
//=> { name: 'Edward Chan', username: 'edwardmchan' }
```

<br>
<hr>
<br>

## ❯ API

### [Enquirer](index.js#L19)

Create an instance of `Enquirer`.

**Params**

* `options` **{Object}**: (optional) Options to use with all prompts.
* `answers` **{Object}**: (optional) Answers object to initialize with.

**Example**

```js
const Enquirer = require('enquirer');
const enquirer = new Enquirer();
```

### [register](index.js#L41)

Register a custom prompt type.

**Params**

* `type` **{String}**
* `fn` **{Function|Prompt}**: `Prompt` class, or a function that returns a `Prompt` class.
* `returns` **{Object}**: Returns the Enquirer instance

**Example**

```js
const Enquirer = require('enquirer');
const enquirer = new Enquirer();
enquirer.register('customType', require('./custom-prompt'));
```

### [prompt](index.js#L79)

Prompt function that takes a "question" object or array of question objects, and returns an object with responses from the user.

**Params**

* `questions` **{Array|Object}**: Options objects for one or more prompts to run.
* `returns` **{Promise}**: Promise that returns an "answers" object with the user's responses.

**Example**

```js
const Enquirer = require('enquirer');
const enquirer = new Enquirer();

(async() => {
  const response = await enquirer.prompt({
    type: 'input',
    name: 'username',
    message: 'What is your username?'
  });
  console.log(response);
})();
```

### [use](index.js#L152)

Use an enquirer plugin.

**Params**

* `plugin` **{Function}**: Plugin function that takes an instance of Enquirer.
* `returns` **{Object}**: Returns the Enquirer instance.

**Example**

```js
const Enquirer = require('enquirer');
const enquirer = new Enquirer();
const plugin = enquirer => {
  // do stuff to enquire instance
};
enquirer.use(plugin);
```

### [use](index.js#L174)

Programmatically cancel all prompts.

**Params**

* `plugin` **{Function}**: Plugin function that takes an instance of Enquirer.
* `returns` **{Object}**: Returns the Enquirer instance.

**Example**

```js
const Enquirer = require('enquirer');
const enquirer = new Enquirer();

enquirer.use(plugin);
```

### [Enquirer#prompt](index.js#L232)

Prompt function that takes a "question" object or array of question objects, and returns an object with responses from the user.

**Params**

* `questions` **{Array|Object}**: Options objects for one or more prompts to run.
* `returns` **{Promise}**: Promise that returns an "answers" object with the user's responses.

**Example**

```js
const { prompt } = require('enquirer');
(async() => {
  const response = await prompt({
    type: 'input',
    name: 'username',
    message: 'What is your username?'
  });
  console.log(response);
})();
```

## Prompt API

<br>
<hr>
<br>

## ❯ Options

### Prompt options

Each prompt takes an options object (aka "question" object), that implements the following interface:

```js
{
  type: string | function,
  name: string | function,
  message: string | function | async function,
  initial: string | function | async function
  format: function | async function,
  result: function | async function,
  validate: function | async function
}
```

| **Property** | **Type** | **Description** |
| --- | --- | --- |
| `type` | `string\|function` | Enquirer uses this value to determine the type of prompt to run, but it's optional when prompts are run directly. |
| `name` | `string\|function` | Used as the key for the answer on the returned values (answers) object. |
| `message` | `string\|function` | The message to display when the prompt is rendered in the terminal. |
| `initial` | `string\|function` | The default value to return if the user does not supply a value. |
| `format` | `function` | Function to format user input in the terminal. |
| `result` | `function` | Function to format the final submitted value before it's returned. |
| `validate` | `function` | Function to validate the submitted value before it's returned. This function may return a boolean or a string. If a string is returned it will be used as the validation error message. |

_(`type`, `name` and `message` are required)._

**Example**

```js
const question = {
  type: 'select',
  name: 'color',
  message: 'Favorite color?',
  initial: 1,
  choices: [
    { name: 'red',   message: 'Red',   value: '#ff0000' },
    { name: 'green', message: 'Green', value: '#00ff00' },
    { name: 'blue',  message: 'Blue',  value: '#0000ff' }
  ]
};
```

### Choice objects

```js
Choice {
  name: string;
  message: string | undefined;
  value: string | undefined;
  hint: string | undefined;
  disabled: boolean | string | undefined;
}
```

| **Property**  | **Type**   | **Description**  |
| --- | --- | --- |
| `name`        | `string`   | The unique key to identify a choice |
| `message`     | `string`   | The message to display in the terminal |
| `value`       | `string`   | An optional value to associate with the choice. This is useful for creating key-value pairs from user choices. |
| `hint`        | `string`   | Value to display to provide user help next to a choice. |
| `disabled`    | `boolean\|string` | Disable a choice so that it cannot be selected. This value may either be `true`, `false`, or a message to display. |

## ❯ Performance

MacBook Pro, Intel Core i7, 2.5 GHz, 16 GB.

### Load time

Time it takes for the module to load the first time (average of 3 runs):

```
enquirer: 4.013ms
inquirer: 286.717ms
prompts: 17.010ms
```

## ❯ Credit

Thanks to [derhuerst](https://github.com/derhuerst), creator of prompt libraries such as [prompt-skeleton](https://github.com/derhuerst/prompt-skeleton), which influenced some of the concepts we used in our prompts.

## About

<details>
<summary><strong>Contributing</strong></summary>

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

</details>

<details>
<summary><strong>Running Tests</strong></summary>

Running and reviewing unit tests is a great way to get familiarized with a library and its API. You can install dependencies and run tests with the following command:

```sh
$ npm install && npm test
```

</details>

<details>
<summary><strong>Building docs</strong></summary>

_(This project's readme.md is generated by [verb](https://github.com/verbose/verb-generate-readme), please don't edit the readme directly. Any changes to the readme must be made in the [.verb.md](.verb.md) readme template.)_

To generate the readme, run the following command:

```sh
$ npm install -g verbose/verb#dev verb-generate-readme && verb
```

</details>

### Contributors

| **Commits** | **Contributor** |  
| --- | --- |  
| 72 | [jonschlinkert](https://github.com/jonschlinkert) |  
| 12 | [doowb](https://github.com/doowb) |  
| 1  | [mischah](https://github.com/mischah) |  
| 1  | [skellock](https://github.com/skellock) |  

### Author

**Jon Schlinkert**

* [GitHub Profile](https://github.com/jonschlinkert)
* [Twitter Profile](https://twitter.com/jonschlinkert)
* [LinkedIn Profile](https://linkedin.com/in/jonschlinkert)

### License

Copyright © 2018, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT License](LICENSE).