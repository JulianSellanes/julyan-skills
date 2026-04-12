# CSS Guide for any AI reading this

version = 1.1

### Naming classes:

1) All classes must be in lowercase, separated by "-" (without spaces)
2) Try to not repeat symbols
3) Do not use "_"
4) Names should be short, usually: component + html element

```css
/* Good: */
.home-div

/* Bad: */
.home__Div
```

### Properties order:

When you create/edit a css class, make sure the properties order is similar to the following example:

```css
.test {
    /* Transform properties */
    flex
    flex-shrink
    width
    max-width
    padding
    margin
    position
    top
    right
    inset
    transform
    z-index
    vertical-align

    /* Display properties */
    display
    flex-flow: row nowrap;
    justify-content
    align-items
    gap
    grid-template-columns

    /* Content properties */
    content
    box-sizing
    overflow
    overflow-x: hidden;
    overflow-y: auto;
    overscroll-behavior-y: contain;
    scroll-behavior
    word-wrap
    overflow-wrap
    white-space

    /* Border properties */
    border
    border-radius
    border-color

    /* Background properties */
    outline
    background
    background-color
    box-shadow

    /* Image properties */
    object-fit
    aspect-ratio
    cursor
    opacity
    visibility
    filter
    mix-blend-mode
    pointer-events
    -webkit-appearance

    /* Text properties */
    color
    font-size
    text-align
    font-family
    font-weight
    line-height
    text-decoration
    text-transform
    list-style
    letter-spacing
    text-shadow
    line-clamp
    accent-color

    /* Transition properties */
    transition
    will-change
    animation
}
```

Note: If one propertie is not listed here, try to add it in a section it could belong

### @media screen

1) These are the allowed resolutions to use.
2) Default is for phones since I like using mobile-first structure
3) They should be placed at the end of the .css file

```css
/* Phones */

/* Default starting point (around 320px) */

/* Small tablets */

@media screen and (min-width: 480px) {}

/* Large tablets */

@media screen and (min-width: 768px) {}

/* Laptops */

@media screen and (min-width: 1024px) {}

/* Desktop */

@media screen and (min-width: 1200px) {}
```

### Pseudo-classes

For pseudo-classes (like links and buttons), follow this pattern whenever possible (hover must be inside @media):

```css
:link    { }
:visited { }
:focus-visible { }
@media (hover: hover) {
    :hover {

    }
}
:active  { }
:disabled { }
```

### All variables must be inside :root

If this is a React+Vite project, it should be inside /frontend/src/index.css
If this is a Nextjs project, it should be inside /frontend/src/globals.css

```css
:root {
    --white: white;
    --black: black;
}
```

### Try not to repeat code/css properties that have already been established in a parent and that affects all its children

For example, if it was declared:

```css
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}
```

Then there's no need to add box-sizing again to a parent (unless it's a very specific and rare case where the box-sizing changes and then it has to be reset), or adding padding/margin: 0
