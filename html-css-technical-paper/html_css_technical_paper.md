# Technical Paper: Fundamentals of HTML and CSS

**Topic:** Web Development \| HTML5 and CSS3

## 1. Introduction

HTML (HyperText Markup Language) and CSS (Cascading Style Sheets) are
the fundamental technologies used to build web pages.

-   **HTML** defines the structure and content of a webpage.
-   **CSS** controls the appearance, layout, and presentation of that
    content.

HTML provides elements such as headings, paragraphs, images, links,
tables, and forms. CSS allows developers to control their colors, sizes,
spacing, positioning, and responsiveness.

Both technologies work together to create websites that are structured,
visually consistent, and accessible across different devices.

## 2. Box Model

The CSS Box Model describes how every HTML element is represented as a
rectangular box. It determines how much space an element occupies on a
webpage.

The Box Model consists of four main parts:

1.  **Content:** The actual content inside an element, such as text or
    an image.
2.  **Padding:** The space between the content and the border.
3.  **Border:** The boundary surrounding the content and padding.
4.  **Margin:** The space outside the border, separating the element
    from other elements.

Example:

``` css
.box {
    width: 200px;
    height: 100px;
    padding: 20px;
    border: 5px solid black;
    margin: 10px;
}
```

By default, CSS uses `content-box`, meaning the declared width and
height apply only to the content.

The total width of the element is:

`200 + 40 + 10 = 250px`

If margins are included, the total horizontal space occupied is:

`250 + 20 = 270px`

### box-sizing

The `box-sizing` property controls how the browser calculates an
element's dimensions.

``` css
* {
    box-sizing: border-box;
}
```

With `border-box`, the declared width includes the content, padding, and
border. This makes layouts easier to calculate and manage.

## 3. Inline versus Block Elements

HTML elements are generally displayed as block-level or inline elements
by default. Their display behavior determines how they occupy space and
interact with surrounding elements.

### 3.1 Block Elements

Block elements normally:

-   Start on a new line.
-   Occupy the available width of their containing block by default.
-   Accept width, height, margin, and padding properties.

Examples:

-   `<div>`
-   `<p>`
-   `<h1>` to `<h6>`
-   `<section>`
-   `<article>`
-   `<header>`
-   `<footer>`

Example:

``` html
<div>First block</div>
<div>Second block</div>
```

The two elements appear on separate lines.

### 3.2 Inline Elements

Inline elements normally:

-   Remain within the same line as surrounding content.
-   Occupy only the space required by their content.
-   Do not behave like block elements when width and height are
    specified.

Examples:

-   `<span>`
-   `<a>`
-   `<strong>`
-   `<em>`
-   `<label>`

Example:

``` html
<p>
    Learn <strong>HTML</strong> and
    <span>CSS</span>.
</p>
```

The inline elements remain within the paragraph's line.

### 3.3 Inline-Block Elements

`inline-block` combines characteristics of both inline and block
elements.

-   Elements remain on the same line when space is available.
-   Width and height can be specified.
-   Padding and margins can be applied.

``` css
.box {
    display: inline-block;
    width: 150px;
    height: 100px;
}
```

**Important:** The `display` property can change an element's default
behavior. For example, a `<div>` can be displayed as inline, and a
`<span>` can be displayed as block.

## 4. CSS Positioning

The CSS `position` property determines how an element is positioned
within a document.

The main positioning values are `static`, `relative`, `absolute`,
`fixed`, and `sticky`.

### 4.1 Relative Positioning

An element with `position: relative` remains in the normal document
flow.

Its position can be adjusted using `top`, `right`, `bottom`, and `left`.

``` css
.box {
    position: relative;
    top: 20px;
    left: 30px;
}
```

The element moves 20 pixels down and 30 pixels to the right from its
original position. Its original space remains reserved in the document.

### 4.2 Absolute Positioning

An element with `position: absolute` is removed from the normal document
flow.

It is positioned relative to its nearest ancestor that has a position
other than `static`. If no such ancestor exists, it is positioned
relative to the initial containing block.

``` html
<div class="container">
    <div class="box">Absolute Element</div>
</div>
```

``` css
.container {
    position: relative;
    width: 300px;
    height: 200px;
}

.box {
    position: absolute;
    top: 10px;
    right: 10px;
}
```

Here, the child element is positioned 10 pixels from the top and right
edges of the container.

### 4.3 Other Positioning Values

  -----------------------------------------------------------------------
  Position                            Description
  ----------------------------------- -----------------------------------
  `static`                            Default positioning; follows normal
                                      document flow.

  `fixed`                             Positioned relative to the viewport
                                      in the usual case and remains in
                                      place while scrolling.

  `sticky`                            Behaves like a relatively
                                      positioned element until a
                                      specified scroll threshold is
                                      reached.
  -----------------------------------------------------------------------

### Relative versus Absolute

  -----------------------------------------------------------------------
  Relative                            Absolute
  ----------------------------------- -----------------------------------
  Remains in normal document flow.    Removed from normal document flow.

  Original space is preserved.        Original space is not preserved.

  Positioned relative to its original Positioned relative to its
  position.                           containing block.
  -----------------------------------------------------------------------

## 5. Common CSS Structural Classes

Structural classes are used to organize a webpage into meaningful
sections and control the overall layout.

These are not built-in CSS classes. They are naming conventions created
by developers.

  Class          Purpose
  -------------- ---------------------------------------------
  `.container`   Wraps and limits the main content width.
  `.wrapper`     Groups related elements together.
  `.header`      Represents the top section of a webpage.
  `.navbar`      Contains navigation links.
  `.main`        Represents the primary content area.
  `.content`     Holds the main content of a section.
  `.sidebar`     Contains secondary content or navigation.
  `.section`     Groups related content.
  `.footer`      Represents the bottom section of a webpage.
  `.row`         Groups elements horizontally.
  `.column`      Represents a vertical content area.

Example:

``` html
<div class="container">
    <header class="header">
        <nav class="navbar">
            Navigation
        </nav>
    </header>

    <main class="main">
        <section class="content">
            Main Content
        </section>

        <aside class="sidebar">
            Sidebar
        </aside>
    </main>

    <footer class="footer">
        Footer
    </footer>
</div>
```

Using meaningful class names makes HTML easier to understand and CSS
easier to maintain.

## 6. Common CSS Styling Classes

Styling classes are used to apply reusable visual properties to HTML
elements.

Like structural classes, these are developer-defined naming conventions
rather than predefined CSS classes.

  Class            Purpose
  ---------------- --------------------------------------
  `.text-center`   Centers text.
  `.text-left`     Aligns text to the left.
  `.text-right`    Aligns text to the right.
  `.text-bold`     Makes text bold.
  `.text-muted`    Applies a less prominent text color.
  `.bg-primary`    Applies a primary background color.
  `.bg-light`      Applies a light background.
  `.border`        Adds a border.
  `.rounded`       Adds rounded corners.
  `.shadow`        Adds a box shadow.
  `.hidden`        Hides an element.
  `.d-flex`        Applies Flexbox display.
  `.d-grid`        Applies Grid display.

Example:

``` css
.text-center {
    text-align: center;
}

.bg-primary {
    background-color: blue;
    color: white;
}

.rounded {
    border-radius: 8px;
}

.shadow {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}
```

These reusable classes reduce repeated CSS declarations and help
maintain consistent styling.

## 7. CSS Specificity

CSS specificity determines which CSS rule is applied when multiple
selectors target the same HTML element.

The main selector categories are:

1.  **Inline styles:** Styles written directly inside an HTML element.
2.  **ID selectors:** Selectors using `#`.
3.  **Class selectors:** Selectors using `.`, including attribute
    selectors and pseudo-classes.
4.  **Element selectors:** Selectors using HTML element names and
    pseudo-elements.

Specificity is commonly represented as a four-part value:

`(a, b, c, d)`

  Selector       Specificity
  -------------- ----------------
  Inline style   `(1, 0, 0, 0)`
  `#header`      `(0, 1, 0, 0)`
  `.container`   `(0, 0, 1, 0)`
  `div`          `(0, 0, 0, 1)`

Example:

``` css
p {
    color: blue;
}

.text {
    color: green;
}

#description {
    color: red;
}
```

``` html
<p class="text" id="description">
    Hello World
</p>
```

The text appears red because the ID selector has higher specificity than
the class and element selectors.

**Important rules:**

-   Higher specificity generally wins when declarations compete in the
    same cascade context.
-   If specificity is equal, the later applicable declaration wins.
-   `!important` changes the cascade priority and should generally be
    avoided unless necessary.
-   CSS cascade layers, scope, and origin also influence which
    declaration wins; specificity is not the only factor.

## 8. CSS Responsive Queries

Responsive web design allows a webpage to adjust its layout and
appearance according to the available screen size and device
characteristics.

CSS Media Queries are used to apply styles under specific conditions.

Syntax:

``` css
@media (condition) {
    /* CSS rules */
}
```

Example:

``` css
.container {
    width: 80%;
    margin: auto;
}

@media (max-width: 768px) {
    .container {
        width: 95%;
    }
}
```

When the viewport width is 768 pixels or less, the container width
changes to 95%.

### Common Breakpoints

  Device category     Example breakpoint
  ----------------- --------------------
  Small mobile                     480px
  Tablet                           768px
  Laptop                          1024px
  Desktop                         1200px

These are common design examples, not mandatory standards. Breakpoints
should be selected based on where the layout needs adjustment.

### Viewport Meta Tag

For responsive layouts to work correctly on mobile browsers, include the
viewport meta tag:

``` html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

This tells the browser to use the device's viewport width and an initial
zoom level of 1.

## 9. Flexbox and CSS Grid

Flexbox and CSS Grid are modern CSS layout systems used to arrange
elements on a webpage.

### 9.1 Flexbox

Flexbox is a one-dimensional layout system. It is primarily used to
arrange elements in a row or a column.

Important properties:

  Property            Purpose
  ------------------- --------------------------------------------------
  `display: flex`     Enables Flexbox.
  `flex-direction`    Defines row or column direction.
  `justify-content`   Aligns items along the main axis.
  `align-items`       Aligns items along the cross axis.
  `flex-wrap`         Allows items to move onto multiple lines.
  `gap`               Defines spacing between items.
  `flex-grow`         Controls how available space is distributed.
  `flex-shrink`       Controls how items shrink when space is limited.
  `flex-basis`        Defines an item's initial main size.

Example:

``` html
<div class="container">
    <div>One</div>
    <div>Two</div>
    <div>Three</div>
</div>
```

``` css
.container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
}
```

The child elements are arranged horizontally with space distributed
between them.

### 9.2 CSS Grid

CSS Grid is a two-dimensional layout system. It allows developers to
control rows and columns simultaneously.

Important properties:

  Property                  Purpose
  ------------------------- ----------------------------------------------------
  `display: grid`           Enables CSS Grid.
  `grid-template-columns`   Defines column sizes.
  `grid-template-rows`      Defines row sizes.
  `gap`                     Defines spacing between rows and columns.
  `grid-column`             Controls an item's column placement.
  `grid-row`                Controls an item's row placement.
  `justify-items`           Aligns items horizontally within their grid areas.
  `align-items`             Aligns items vertically within their grid areas.

Example:

``` css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}
```

This creates three equally sized columns.

### 9.3 Flexbox versus Grid

  -----------------------------------------------------------------------
  Flexbox                             Grid
  ----------------------------------- -----------------------------------
  One-dimensional layout.             Two-dimensional layout.

  Works primarily with rows or        Works with rows and columns
  columns.                            together.

  Useful for navigation bars and      Useful for page layouts,
  aligned components.                 dashboards, and galleries.

  Content-driven layout is common.    Grid-track-driven layout is common.
  -----------------------------------------------------------------------

Both systems can be used together in the same webpage. The choice
depends on the layout requirements.

## 10. Common HTML Header Meta Tags

The `<head>` element contains metadata and references to external
resources used by the webpage. Its contents are generally not displayed
directly in the page body.

### 10.1 Character Encoding

``` html
<meta charset="UTF-8">
```

Specifies the character encoding used by the document. UTF-8 supports a
wide range of characters and symbols.

### 10.2 Viewport

``` html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Controls the viewport dimensions and initial scale, which is important
for responsive layouts.

### 10.3 Page Description

``` html
<meta name="description" content="Learn HTML and CSS fundamentals.">
```

Provides a short description of the webpage. Search engines may use this
information when generating search-result snippets.

### 10.4 Page Author

``` html
<meta name="author" content="Your Name">
```

Specifies the author of the webpage.

### 10.5 Page Title

``` html
<title>HTML and CSS Technical Paper</title>
```

Defines the title displayed in the browser tab and commonly used in
search results.

### 10.6 External CSS

``` html
<link rel="stylesheet" href="style.css">
```

Connects an external CSS stylesheet to the HTML document.

### Complete Example

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">

    <meta name="description"
          content="HTML and CSS learning material.">

    <meta name="author" content="Your Name">

    <title>HTML and CSS Fundamentals</title>

    <link rel="stylesheet" href="style.css">
</head>
<body>

</body>
</html>
```

## 11. HTML Semantic Elements

Semantic HTML means using HTML elements according to their meaning and
purpose rather than choosing elements only for their visual appearance.

Semantic elements help browsers, search engines, screen readers, and
developers understand the structure of a webpage.

### Common Semantic Elements

  -----------------------------------------------------------------------
  Element                             Purpose
  ----------------------------------- -----------------------------------
  `<header>`                          Represents introductory content or
                                      a page/section header.

  `<nav>`                             Contains navigation links.

  `<main>`                            Represents the primary content of a
                                      webpage.

  `<section>`                         Groups related content into a
                                      section.

  `<article>`                         Represents independent,
                                      self-contained content.

  `<aside>`                           Contains indirectly related or
                                      supplementary content.

  `<footer>`                          Represents footer information.

  `<figure>`                          Groups self-contained content, such
                                      as an image or diagram.

  `<figcaption>`                      Provides a caption for a figure.
  -----------------------------------------------------------------------

Example:

``` html
<body>
    <header>
        <h1>My Website</h1>
    </header>

    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
    </nav>

    <main>
        <section>
            <h2>Latest Articles</h2>

            <article>
                <h3>Introduction to HTML</h3>
                <p>HTML structures web content.</p>
            </article>
        </section>

        <aside>
            Related information
        </aside>
    </main>

    <footer>
        Copyright Information
    </footer>
</body>
```

**Why Semantic HTML is Important**

-   Improves code readability and maintainability.
-   Helps assistive technologies understand page structure.
-   Provides meaningful information to search engines.
-   Makes webpages easier to navigate and organize.

Using semantic elements appropriately is preferable to using `<div>` for
every section.

## 12. CSS Selectors

CSS selectors are patterns used to identify HTML elements to which CSS
rules should be applied.

### 12.1 Element Selector

Targets all elements with a particular HTML tag.

``` css
p {
    color: blue;
}
```

This applies blue text to all paragraph elements.

### 12.2 Class Selector

Targets elements containing a particular class name.

``` css
.container {
    width: 80%;
}
```

HTML:

``` html
<div class="container">Content</div>
```

A class can be reused across multiple HTML elements.

### 12.3 ID Selector

Targets an element with a specific ID.

``` css
#header {
    background-color: black;
}
```

HTML:

``` html
<header id="header">Website Header</header>
```

An ID should be unique within an HTML document.

### 12.4 Universal Selector

Targets all elements.

``` css
* {
    margin: 0;
    padding: 0;
}
```

### 12.5 Grouping Selector

Applies the same styles to multiple selectors.

``` css
h1, h2, p {
    font-family: Arial, sans-serif;
}
```

### 12.6 Descendant Selector

Targets elements located anywhere inside another element.

``` css
.container p {
    color: green;
}
```

This targets all paragraph elements inside `.container`, including
nested descendants.

### 12.7 Child Selector

Targets only direct children of an element.

``` css
.container > p {
    color: red;
}
```

### 12.8 Pseudo-Classes

Pseudo-classes select elements based on their state or position.

``` css
a:hover {
    color: red;
}

input:focus {
    border-color: blue;
}

li:first-child {
    font-weight: bold;
}
```

Common pseudo-classes include:

-   `:hover`
-   `:focus`
-   `:active`
-   `:first-child`
-   `:last-child`
-   `:nth-child()`
-   `:checked`
-   `:disabled`

### 12.9 Pseudo-Elements

Pseudo-elements allow developers to style specific parts of an element
or insert generated content.

``` css
p::first-letter {
    font-size: 24px;
}

p::before {
    content: "Note: ";
}

p::after {
    content: " ✓";
}
```

Common pseudo-elements include `::before`, `::after`, `::first-letter`,
`::first-line`, and `::placeholder`.

## 13. CSS Units

CSS units define measurements for properties such as width, height, font
size, margin, and padding.

There are two main categories: absolute units and relative units.

### 13.1 Absolute Units

Absolute units represent fixed measurements.

  Unit   Description
  ------ -------------
  `px`   CSS pixel.
  `cm`   Centimeter.
  `mm`   Millimeter.
  `in`   Inch.
  `pt`   Point.
  `pc`   Pica.

Example:

``` css
.box {
    width: 300px;
    height: 150px;
}
```

Pixels are commonly used for borders, small spacing values, and other
measurements requiring precise control.

### 13.2 Relative Units

Relative units depend on another measurement, such as the font size of
an element, the root font size, or the dimensions of a containing block
or viewport.

  -----------------------------------------------------------------------
  Unit                                Description
  ----------------------------------- -----------------------------------
  `%`                                 Relative to a relevant containing
                                      dimension or property reference.

  `em`                                Relative to the computed font size
                                      of the element (with
                                      context-dependent inheritance).

  `rem`                               Relative to the root element's font
                                      size.

  `vw`                                1% of the viewport width.

  `vh`                                1% of the viewport height.

  `vmin`                              1% of the smaller viewport
                                      dimension.

  `vmax`                              1% of the larger viewport
                                      dimension.
  -----------------------------------------------------------------------

Example:

``` css
html {
    font-size: 16px;
}

.container {
    width: 80%;
    padding: 2rem;
}

.heading {
    font-size: 2rem;
}
```

Here, `2rem` is equivalent to 32px because the root font size is 16px.

**Recommendation:** Use relative units such as `rem`, `em`, and
percentages where flexibility and responsive behavior are important. Use
fixed units when a fixed measurement is appropriate.

## 14. CSS Variables (Custom Properties)

CSS variables allow developers to store reusable values and use them
throughout a stylesheet.

They are particularly useful for maintaining consistent colors, spacing,
and other design values.

Example:

``` css
:root {
    --primary-color: blue;
    --secondary-color: gray;
    --main-spacing: 20px;
}

button {
    background-color: var(--primary-color);
    padding: var(--main-spacing);
}

p {
    color: var(--secondary-color);
}
```

The `:root` selector defines variables at the document root level,
making them available throughout the document.

The `var()` function retrieves the value of a custom property.

**Advantages:**

-   Reduces repeated values.
-   Makes global design changes easier.
-   Improves CSS maintainability.
-   Supports consistent styling across components.

## 15. HTML Forms and Input Elements

HTML forms allow users to submit information to a website or
application.

The `<form>` element acts as a container for input fields and other form
controls.

### Common Form Elements

  Element        Purpose
  -------------- ----------------------------------------
  `<form>`       Defines a form.
  `<input>`      Accepts different types of user input.
  `<label>`      Provides a description for an input.
  `<textarea>`   Accepts multi-line text.
  `<select>`     Creates a dropdown list.
  `<option>`     Defines an option inside a dropdown.
  `<button>`     Creates a clickable button.

Example:

``` html
<form action="/submit" method="post">

    <label for="username">Username:</label>
    <input type="text" id="username" name="username">

    <label for="email">Email:</label>
    <input type="email" id="email" name="email">

    <button type="submit">Submit</button>

</form>
```

### Common Input Types

-   `text`
-   `password`
-   `email`
-   `number`
-   `date`
-   `radio`
-   `checkbox`
-   `file`
-   `submit`

HTML also supports basic form validation through attributes such as
`required`, `min`, `max`, `minlength`, and `maxlength`.

## 16. CSS Transitions and Transformations

CSS transitions and transformations are used to create visual effects
and modify how elements appear or move.

### 16.1 Transitions

Transitions allow CSS property changes to happen gradually over a
specified duration.

Example:

``` css
button {
    background-color: blue;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: green;
}
```

When the user moves the pointer over the button, its background color
changes gradually.

Common transition properties:

-   `transition-property`
-   `transition-duration`
-   `transition-timing-function`
-   `transition-delay`

### 16.2 Transformations

The `transform` property allows elements to be rotated, scaled, moved,
or skewed without changing their normal document-flow position.

Example:

``` css
.box {
    transform: translateX(20px);
}
```

Common transform functions:

  Function        Purpose
  --------------- ---------------------------------
  `translate()`   Moves an element.
  `rotate()`      Rotates an element.
  `scale()`       Changes the size of an element.
  `skew()`        Skews an element.

Example:

``` css
.box:hover {
    transform: scale(1.1);
}
```

This increases the element's visual size when hovered.

## 17. HTML Accessibility Basics

Web accessibility means designing webpages so that people with different
abilities can use and understand them.

Accessibility should be considered while writing HTML and CSS rather
than added only after development.

Important practices include:

1.  Use semantic HTML elements wherever appropriate.
2.  Add descriptive `alt` attributes to meaningful images.
3.  Associate form labels with their input fields.
4.  Maintain sufficient contrast between text and background colors.
5.  Use headings in a logical hierarchy.
6.  Ensure interactive elements can be accessed using a keyboard.
7.  Provide visible focus indicators.
8.  Do not communicate important information through color alone.

Example:

``` html
<img src="profile.jpg" alt="Profile photograph">

<label for="email">Email Address</label>
<input type="email" id="email">

<button type="submit">Submit</button>
```

Accessibility improves usability and makes websites more inclusive.

## 18. CSS Overflow and Z-Index

These properties help manage content that exceeds an element's
dimensions and control how overlapping elements are displayed.

### 18.1 Overflow

The `overflow` property controls what happens when content is larger
than its containing element.

Common values:

  Value       Description
  ----------- -----------------------------------------
  `visible`   Content can extend outside the element.
  `hidden`    Excess content is clipped.
  `scroll`    Displays scrolling mechanisms.
  `auto`      Adds scrolling when necessary.

Example:

``` css
.container {
    width: 200px;
    height: 100px;
    overflow: auto;
}
```

### 18.2 Z-Index

The `z-index` property controls the stacking order of overlapping
elements within their stacking contexts.

Example:

``` css
.box-one {
    position: absolute;
    z-index: 1;
}

.box-two {
    position: absolute;
    z-index: 2;
}
```

Here, `.box-two` appears above `.box-one` when they overlap in the same
stacking context.

A higher `z-index` does not necessarily place an element above every
other element on the page because stacking contexts affect how elements
are layered.

## 19. Best Practices for HTML and CSS

Following good development practices makes webpages easier to maintain,
understand, and expand.

### HTML Best Practices

1.  Use semantic HTML elements.
2.  Maintain proper indentation and formatting.
3.  Use meaningful class and ID names.
4.  Keep the HTML structure organized.
5.  Include appropriate alternative text for images.
6.  Use external stylesheets for reusable styling.
7.  Avoid unnecessary HTML elements.

### CSS Best Practices

1.  Prefer Flexbox and Grid for layout instead of using positioning for
    everything.
2.  Use `box-sizing: border-box` for predictable element dimensions.
3.  Prefer reusable classes over repeating styles.
4.  Use relative units where appropriate.
5.  Avoid unnecessary use of `!important`.
6.  Keep CSS selectors simple.
7.  Organize styles into logical sections.
8.  Test layouts at different viewport sizes.
9.  Use CSS variables for reusable values.
10. Follow accessibility guidelines.

## 20. Conclusion

HTML and CSS are the foundation of modern web development. HTML provides
the structure and meaning of webpage content, while CSS controls its
appearance and layout.

Understanding the Box Model, display properties, positioning, selectors,
specificity, Flexbox, Grid, and responsive design is essential for
building effective web layouts.

Semantic HTML, accessibility, reusable CSS, and consistent coding
practices help developers create maintainable and user-friendly
websites.

A strong understanding of these concepts provides the foundation for
learning advanced CSS techniques and frontend frameworks.

## 21. References

1.  **MDN Web Docs -- HTML: HyperText Markup Language**\
    https://developer.mozilla.org/en-US/docs/Web/HTML

2.  **MDN Web Docs -- CSS: Cascading Style Sheets**\
    https://developer.mozilla.org/en-US/docs/Web/CSS

3.  **MDN -- CSS Box Model**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model

4.  **MDN -- CSS Positioning**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/position

5.  **MDN -- CSS Specificity**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity

6.  **MDN -- CSS Media Queries**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries

7.  **MDN -- Responsive Web Design**\
    https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design

8.  **MDN -- Flexbox**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout

9.  **MDN -- CSS Grid**\
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout

10. **MDN -- HTML Meta Element**\
    https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta

11. **MDN -- HTML Semantics**\
    https://developer.mozilla.org/en-US/docs/Glossary/Semantics

12. **W3C -- Web Accessibility Initiative**\
    https://www.w3.org/WAI/
