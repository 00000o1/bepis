<p align=center>
  <img src="readme-images/bepis-logo.jpg?raw=true" alt="Bepis Logo">
</p>

# [bepis](#drincc) ![download badge](https://img.shields.io/npm/dw/bepis) ![version badge](https://img.shields.io/npm/v/bepis/latest)

Bepis is a crazy new way to write dynamic HTML + CSS in JavaScript.

[It Is On Npm](https://www.npmjs.com/package/bepis)

You can use [snowpack](https://github.com/pikapkg/snowpack).

## Examples

Simple keyed list, play with it [here](https://codesandbox.io/s/bepis-latest-playground-6cggy):

```javascript
import { w, clone } from "bepis";

// setup
const myItems = [
  { name: "Ratchet", description: "Tool", key: "z2" },
  { name: "Screw", description: "Part", key: "a3" },
  { name: "Sand", description: "Material", key: "p4" },
  { name: "Moxie", description: "Intangible", key: "x5" },
  { name: "Evie", description: "Name", key: "s1" }
];
const newName = "Mojo";

// keyed component
const Item = item => w` ${item.key} 
  li p, 
    :text ${item.description}.
    a ${{ href: item.url }} :text ${item.name}..`;

// singleton component
const List = items => w` ${true} ul :map ${items} ${Item}`;

// mount
List(myItems)(document.body);

// change
const myChangedItems = clone(myItems);
myChangedItems[3].name = newName;

// see update
setTimeout(() => List(myChangedItems), 2000);
```

## Get

```
$ npm i bepis
```

## TCM

*Traditional Chinese Medicine?* This time no. **:text, :map and :comp** directives.

- Use `:text` to insert text nodes that have siblings. Use `.innerText` in the first param to set text without siblings.

  ```javascript
    w`
      h1,
        :text ${"Bepis is "}.
        em ${"REALLY"}.
        :text ${" the best!"}.
        :text ${" YES"}
    `(document.body);
  ```
  
- Use `:comp` to insert another "Component": a function returning an Element or String.

  ```javascript
    const Spinner = () => w`i ${{class:'fa-spinner', hidden:true}}`;
    w`
    button,
      :text ${"Save"}.
      :comp ${data} ${Spinner}
    `
  ```
  - If first param is a function it is called to get the input to pass to second param.
  - Otherwise first param is passed to second param. The result inserted.
  - If second param is omitted, first param is a function. Its result inserted.
  
- Use `:map` to insert multiple.

  ```javascript
    const dataList = [{name:'He'}, {name:'Si'}, {name:'Kr'}];
    const ItemBepis = item => w`li h1 ${item.name}`;
    w`
    ul,
      :map ${dataList} ${ItemBepis}
    `
  ```
  - The second parameter can be omitted when each list member is an Element or a String.

## Basics

- Use template literals tagged with `w`. This creates a 'bepis'
- Use ',' operator to save an insertion point
- Use '.' operator to load an insertion point
- After a tag path the first parameter is the content (string or Element properties object)
- After a tag path the second parameter is the style (inline style object scoped to that element)
- A tagged template literal returns an insertion function. Call that function with the Element you want to append this markup to.
- Whitespace in the template literal has no special meaning and, except to separate tags, is ignored.
- If you want to use the style parameter, but not the content parameter you need to put a null or undefined in the content parameter. I do this in the examples by using a variable set to null.
- The last sequence of '.' operators in a bepis can **not** be omitted, otherwise those nodes will not be inserted.

## Component types

- Pinned: singleton, specify with first parameter equal `true`, e.g.
  ```
  const print1 = () => w`p label input ${{value:count++}}`
  const print2 = () => w`${true} p label input ${{value:count++}}`
  // equivalent
  ```
- Pinned: instance, specify with first parameter a key that identifies, e.g.
  ```
  const print = key => w`${key} p label input ${{value:count++}}`
  ```
- Free: multiple (the default), specify with no first parameter or first parameter false, e.g.
  ```
  const print = () => w`${false} p label input ${{value:count++}}`
  ```

After mounting, a singleton component will always update in place when its bepis is called.
Similarly, each instance component will update in place when its bepis is called with its key.
Free components, create new markup, and must be mounted every call.

## Up next

- minimal diffing with updator functions.

## Related Projects

I don't know. I didn't search any "prior art." Let me know how I reinvented this beautiful wheel by opening a PR request.


----------


<p align=center>
  <img src="readme-images/bepiswatnsyou.jpg?raw=true" alt="Bepis Wants You" width="80%">
</p>
