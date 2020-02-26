# PostCSS Retina 1px 

[PostCSS] plugin for transform 1px retina border by scale-box on mobile.

* Automatically transform your border in retina without change anything.  
* support border-radius and color.
* it's easy to remove it to use other way such as [lib-flexible](https://github.com/amfe/lib-flexible)

## How It Works
use a **```::before```** element to create a scale border and replace the origin.


## Example

before:

```css
.foo {
  color: white;
  border: 1px solid red;
  border-radius: 6px;
}
```  

after:

```css
.foo {
  color: white;
  position: relative; 
}

.foo::before {
  border: 1px solid red;
}

.foo::before {
  content: ' ';
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  pointer-events: none;
  box-sizing: border-box;
  font-size: 100px;
}

/* dpr is 2 */

@media only screen and (-webkit-min-device-pixel-ratio: 2), 
only screen and (min-device-pixel-ratio: 2) {
  .foo::before {
    transform: scale(0.5);
    transform-origin: 0 0;
    width: 200%;
    height: 200%;
    font-size: 200px;
    border-radius: 12px;
  }
}

/* dpr is 3 */

@media only screen and (-webkit-min-device-pixel-ratio: 3), 
only screen and (min-device-pixel-ratio: 3) {
  .foo::before {
    transform: scale(0.333333);
    transform-origin: 0 0;
    width: 300%;
    height: 300%;
    font-size: 300px;
    border-radius: 18px;
  }
}
```

## Install
```
npm install --save-dev postcss-retina-1px
```

## Usage

```js
postcss([ require('postcss-retina-1px') ])
```

Default Options:
```js
{
  radiusUnit: 'px' /*border-radius unit（only deal with this unit）*/
}
```

## Notice

* make sure your box is not a pseudo-class.
```css
/* WILL NOT to be transformed */
.wrong::before {
  border: 1em;
}
/* WILL NOT to be transformed */
.wrong::after {
  border: 1em;
}
```
* make sure your **```border```** use **```1px```**
```css
/* WILL be transformed */
.right {
  border: 1px;
}
/* WILL NOT be transformed */
.wrong {
  border: 1em;
}
```

* **```border-radius```** will not be transformed if there is not a border declare in same rule.  
```css
.foo {
  border: 1px solid red;
}
.bar {
  /* radius WILL NOT to be transformed */
  border-radius: 2px;
}
```
```css
.foo {
  border: 1px solid red;
  /* radius WILL to be transformed */
  border-radius: 2px;
}
```

* if your don't need transform any border, try to do like: 

```css
.foo {  
  border: 1px solid red; /*no*/ 
}
```

See [PostCSS](https://github.com/postcss/postcss) docs for examples for your environment.
