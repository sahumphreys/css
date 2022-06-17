# CSS Notes (Intermediate)

## The Box Model

- Everything displayed by a CSS is a __box__.
- The behaviour of the box changes based on:
  -  the ```display``` value
  -  the dimensions set
  -  the content inside
-  __extrinsic sizing__: the size of the box is fixed
-  __intrinsic sizing__: the size is determined by the browser, based on the size of the content
-  Better to use intrinsic sizing by either:
   -  unset the width, or
   -  set ```width``` to be ```min-content```  (Other attributes includ ```max-content``` and ```fit-content```)


### Areas of the box model

![](areas_of_the_box_model.svg)

- __content__: where the content lives! This can control the size of the parent
- __padding__: surrounds the content box
- __border__: surround the padding, it's the edge of the box
- __margin__: the space around the box

### Controlling the box model

- Every browser applies a user-agent stylesheet to HTML, defines how elements look in the absence of a style sheet.  Differ between browsers.


NB
__Block Level vs Inline Level__
- Block level elements take up as much space as possible.  Each starting a new line, also occupying as much horizontal space as possible E.g. ```p```, ```ol```, ```ul```, ```li```, headings, ```article```, ```section```, ```div```
- Inline elements display in a line, they do not force the text after them to a new line E.g. ```a```, ```strong```, ```em```, ```b```, ```i```,```q```, ```mark```, ```span```

## Selectors

Define the element to which the CSS rules apply.

- E.g. a first paragraph might need to be larger than remaining paragraphs e.g.

~~~~~{html}
<p>Here is the first paragraph and we want it to stand out in some way</p>
<p>While the second and subsequent paragraphs can be made to conform to the standard styling</p>
~~~~~

- A __selector__ can be used to find the specific element and apply a CSS rule:

~~~~~{css}
article p:first-of-type {
    color : #CC0000;
    font-size: 1.5em;
}
~~~~~

- The __universal selector__, ```*``` can be applied to every element.
- A __type selector__, matches an HTML element e.g.

~~~~~{css}
section {
  padding : 2em;
}
~~~~~

- A __class selector__ matches any element with that class applied to it
- An __id selector__ matches any element with that id applied to it
- An __attribute selector__ matches any element with the given attribute e.g.

~~~~~{css}
[data-type="primary"] {
  color : green;
}
~~~~~

~~~~~{html}
<div data-type="primary"></div>
~~~~~

- This can be used to match a particular attribute even if no value is given.
- A __matching selector__(?) can be used to match a portion of text e.g. a link that starts "https":

~~~~~{css}
[href^='https] {
  color : blue;
}
~~~~~

- Use ```*``` and it will match where that string occurs
- Selectors can be grouped, separated by commas before defining them.

### Pseudo-classes

- These focus on specific state e.g. when the mouse hovers over a button, or parts of an element:

~~~~~{css}
/* link changes when being hovered over */
a:hover {
  outline: 1px dotted blue;
}
/* All even paras have a different background */
p:nth-child(even) {
  background-color : #e7e7e7;
}
~~~~~

### Pseudo-elements

- Act a if they are inserting a new element with CSS.  
- They use ```::``

~~~~~{css}
.my-element::before {
  content : 'Prefix -';
}
/* can also use ::after */
~~~~~

- Also, e.g., highlight area selected by a user:

~~~~~{css}
::selection {
  background : black;
  color : white;
}
~~~~~

(Read more here: [https://web.dev/learn/css/pseudo-elements](https://web.dev/learn/css/pseudo-elements))

### Complex selectors

- Selectors can be grouped using a __combinator__
- A __combinator__ sits between two selectors
- They help select items based on their position in the document
- A __selector list__ groups elements when separated by a comma, ```,```.  E.g. ```div, span {}``` will match both the div and span elements
- A __descendant combinator__ targets a _child_ element and uses a space, "``` ```":

~~~~~{css}
p strong {
  color : green;
}
~~~~~

- This is different to the selector list!
- All ```<strong>``` elements inside ```<p>``` will be green.
- This is recursive, so ALL such elements will be styled similarly
- A __child combinator__, ```>```, selects elements that are direct children of the first e.g. ```ul > li {}``` will match all ```li``` elements nested inside a ```ul```.
- A __next sibling container__ looks for an element immediately following another using ```+``` in the selector

(See: [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors))

## Cascade

There may be multiple rules for an element, cascade is the algorithm that determines which one applies.  There are four stages:

- __Position and order of appearance__: order in which they appear
- __Specificity__: strongest match e.g. class rules are more specific than ```h``` rules wherever they appear, an ```id``` is even more specific (best not to attach styles to ```id```s)
- __Origin__: when it appears and where it comes from
- __Importance__: some rules have a greater weight, or include the ```!important``` rule

## Specificity

E.g.

~~~~~{html}
<button class="branding">Hello, Specificity!</button>
~~~~~

~~~~~{css}
button {
  color: red;
}

.branding {
  color: blue;
}
~~~~~

- Q. What colour will the button be?
- A: Blue.  The class is more __specific__
- Under the hood, each selector rule is givena number of points, the higher the score the more specific:
  - Universal selector, ```*```, gets $0$
  - Element, or pseudo-element, e.g. ```div```, gets $1$
  - Class, or pseudo-class, or attribute e.g. ```.my-class```, ```a:hover```, gets $10$
  - Id selector e.g. ```#myID``` gets $100$
  - Inline style attribute, using ```style``` attribute of an element gets $1000$ points
  - The ```!important``` rules gets $10000$

## Inheritance

Some CSS properties inherit values if they are not set using a CSS rule.

- Add a CSS property to the ```html``` element, say ```color``` and all elements that can inherit that property will have their color set to the same value
- Add say a ```font-size``` rule to the ```body``` and elements will inherit this too, though not ````html``` as it cascades downwards.
- Not all properties are inheritable but a lot are.
- If a parent has say ```font-weight``` set, then child elements will automatically inherit that value
- You can force an element to take the inherited value using ```inherit```
- Values can be 'reset' using ```initial```, setting the property back to its default value

NB. One of the benefits is setting 'site-wide' styles e.g.

~~~~~{css}
* {
  font-family: inherit;
  line-height: inherit;
  color: inherit;
}

html {
  font-size: 125%;
  font-family: sans-serif;
  line-height: 1.5;
  color: #222;
}
~~~~~

(See: [https://www.smashingmagazine.com/2016/11/css-inheritance-cascade-global-scope-new-old-worst-best-friends/](https://www.smashingmagazine.com/2016/11/css-inheritance-cascade-global-scope-new-old-worst-best-friends/)

- There is no need now to set any of these values to subsequent elements, unless needed to set alternative values.
- So style all the elements you're going to need as generally as possible, think of designing the element for the whole of the interface
- For each new component, if it introduces new elements, style them with element selectors
- Potentially preferable to writing classes

## Color (Colour)

- __Hexcodes__:  e.g. ```color: #3498B3```. Can also add a 4th pair for __alpha value__ (transparency) as a percentage (in Hex, so 80 is 50%, BF is 75%)
- __RGB__: e.g. ```color: rgb(58, 200, 123);```.  Alpha value added: ```color: rgb(58, 200,123)/50%```.  Modern browsers dispense with the commas.
- __HSL__:  (Hue Saturation Lightness) e.g. ```color: hsl(344, 79%, 34%);```.  Hue, value from 0 to 360 on a colour wheel (both 0 and 360 are red).  Saturation is for the vibrancy of the colour, 0% will be greyscale.  Lightness, the scale of added light where 100% will be white.  Alpha is added as for RGB colours.
- __Keywords__: There are 148 of them

## Sizing Units

