# Egghead: Build Modern Layouts with CSS Grid

Notes on this Egghead course. <https://egghead.io/courses/build-modern-layouts-with-css-grid-d3f5>

## Lesson 1: Define Root Colors Variables and Set Up Box-Sizing for a CSS Layout

Mostly just copy and paste code from Github to start.

## Lesson 2: Introduction to CSS Rows and Columns with `display: grid`

Use `display: grid` to create a CSS Grid container (similar to `display: flex`).

Use `grid-template-columns` with numbers like `100px 100px 100px` to specify the width of each column.
Use `grid-template-rows` with numbers like `75px 75px 75px` to specify the height of each row.

## Lesson 3: Specify a Gutter in CSS Grid with `grid-gap`

By default, if CSS Grid items have a border, elements with a `border` that border another element with a `border` will have a border that looks twice as thick as the actual border, because each item's border is being applied. You can space elements with `grid-column-gap` and `grid-row-gap`. The shorthand for those properties is `grid-gap`. If you use `grid-gap`, you can give it two values. The first is `row-gap` and the second is `column-gap`.

## Lesson 4: Utilize Firefox Developer Tools to Visualize CSS Grid Styles

FF Developer dev tools are best of CSS Grid troubleshooting (Chrome isn't too bad, assume they're improving this).

## Lesson 5: Determing CSS Grid Sizing with the Fraction Unit (`fr`)

`grid-template-columns` can take percentage units like `25% 25% 25% 25%` instead of `100px 100px 100px 100px`. But percentage units don't take into account `grid-column-gap`, so if there's any gap, the 100% gtc will take up more than 100% of the screen width.

Percentage units are responsive. Pixel units are not (duh?).

We can use fraction units (`fr`) and let the browser do the math for us. It distributes the "remaining" space after manually set units like `px` and `%` are accounted for, and distributes that pro rata.

You can mix and match units. E.g., `grid-template-column: 50% 1fr 1fr 1fr`. The first column takes up 50% of the screen width. Each of the last three columns will take up slightly less than 1/6 of the width of the screen. All elements will fit on the screen, even with `grid-column-gap`. With `g-t-c: 50% 1fr 2fr 1fr`, the second column from the right will be twice as wide as its neighbors.

## Lesson 6: Define a Page Layout with CSS Grid Using `grid-template-areas`

This is in `index2.html`.

`grid-template-areas` lets you spatially define how you want certain objects to appear on your screen. It looks like a grid as you define columns and rows. With

```CSS
grid-template-areas:
"header"
"content"
"footer"
```

You can look on the screen and see those content areas named (both in FF and Chrome).

If we want a Nav to left of content area (left sidebar), we can use

```CSS
grid-template-areas:
"header header"
"nav content"
"footer footer"
```

Without changes to our DOM, elements are just put in their natural position. Row 1 is `Header` and `Content`. Row 2 is `Footer`. Other spaces in our grid are blank.

To fix this, (1) give each DOM element a distinct class name (doesn't matter what). (2) Give each newly-created DOM className a styling of `grid-area` that matches their position in the `grid-template-areas`. In our case, `<div class="contentMcContentFace item>Content</div>` and `.contentMcContentFace { grid-area: content }`. N.B. `grid-area`'s name does **not** take quotes like the names in `grid-template-areas` do.

## Lesson 7: Align Content by Adding Stying to a CSS Grid Layout

Vertical-axis = "block-axis". Takes `align-items`.
Needs element to have `display: grid`.
Default positioning is at the top. Can also be done manually with `align-items: start`.
`align-items: end` takes content to the bottom of the grid.
`align-items: center` will vertically center content.

Horizontal axis: "inline-axis". Takes `justify-items`.
Also takes `start`, `center`, and `end` as options.
Default positioning is `start`/left.

## Lesson 8: Building a Navigation Bar with CSS Grid Using `grid-auto-flow`

Intent: `header` has a Logo on the left side and some nav links on the right side.

Adding `display: grid` to an element means all of its children will automatically be displayed vertically.
Use `grid-auto-flow: row` to use default vertical display.
Use `grid-auto-flow: column` to get them to display horizontally. (WTF? This seems backwards.)
Adding `grid-template-columns: 1fr` makes items behavior like we want them to. (WTF? Is it because `<a>` tags are inline and don't create their own space and `<p>` does? This makes no sense.)

Also added `padding`, `grid-gap`, and `align-items: center` for presentational reasons.

### My Explanation

Going to <https://developer.mozilla.org/en-US/docs/Web/CSS/grid-auto-flow>, the explanation for `grid-auto-flow: row` is "Items are placed by filling each row in turn, adding new rows as necessary." So with 3 columns and 2 rows, elements get placed like

A-C-E
B-D-F

Using `grid-auto-flow: columns` instead, the alignment is

A-B-C
D-E-F

The behavior for the specified direction of `grid-auto-flow` is controlled by the `grid-template-` property for that direction. Any `grid-template` property for the other direction will be ignored (or so it seems from my brief testing).

## Leeson 9: Create a Nested CSS Grid Layout

Create a new `portfolio` element in HTML and give it some portfolio items, class of `portfolio_item item`. Add `"portfolio"` to `grid-areas`. Give `.portfolio` a `display: grid` to make it a grid and `grid-template-columns: 1fr 1fr 1fr` so you have three columns. Add some `padding` and `grid-gap` for presentation.

## Lesson 10: Use the `repeat()` CSS Function to Efficiently Define CSS Grid Rows and Columns

Instead of using `grid-template-columns: 1fr 1fr 1fr 1fr 1fr`, you can use `grid-template-columns: repeat(5, 1fr)`.

N.B. VSCode's auto-fill has a space in `repeat ()`. There should not be any space.

You can use different widths for properties with `grid-template-columns: repeat (2, 1fr 2fr)`. In this case, there will be 4 columns in a row. The 1st/3rd columns will have width 1fr, and 2nd/4th will have width 2fr.

## Lesson 11: Define Size Range with the `minmax()` CSS Function to Create Responsive Grid Items

Grids are automatically responsive to the screen size, but we may only want that to go so far (right now, down to the limit of the narrowest row).

In `.portfolio`, we can change `grid-template-columns` from `repeat(5, 1fr)` to `repeat(5, minmax(150px, 1fr))`. In this case, there will be 5 columns, each at least 100px wide and they'll occupy all the extra space proportionally. Without enough space to fit the min width, there wil be a horizontally scrollbar on the bottom of the screen.

To address this issue, change to `repeat(auto-fit, minmax(150px, 1fr))`. This creates a dynamic number of columns based on the screen width.

*My question*: Is there a way we can give a maximum number of columns? Replace `minmax(150px, 1fr)` with `minmax(150px, 250px)`? This works in a way, but eventually breaks down. Items will automatically live at their maximum space, and only at their maximum space instead of decreasing to fill the screen (so at 850px, you'll have A-B-C-100px of blank space). Can we have both things?

## Lesson 12: Create a Responsive Layout Using Media Queries with CSS Grid

Goal: content and profile area (to left of content) should be adjacent to each other until screen reaches a certain size, and then they should be stacked vertically.

Solution:

```CSS
@media (max-width: 700px) {
  .container {
    grid-template-columns: 1fr;
    grid-template-areas:
    "header"
    "profile"
    "content"
    "portfolio"
    /* STANDARD
    grid-template-columns: 1fr 2fr;
    grid-template-areas:
    "header header"
    "profile content"
    "portfolio portfolio"
    */
  }
}
```

## Exercise 13: Use grid-auto-flow and Media Queries to Flip Navigation from Horizontal to Vertical

Goal: similar to #12, but for the Nav links in the Header to avoid cramping. Change navigation from horizontal to vertical alignment once the screen size reaches a certain width.

Solution:

```CSS
@media (max-width: 500px) {
  .header {
    grid-auto-flow: row;
    grid-gap: 5px;
  }
}
```

N.B. the media query must come after the class that it's changing. So `@media (max-width: 500px) { .header { ... }}` must be after `header` in the CSS.

Downside: when content switches from horizontal orientation to vertical orientation, it may overflow its container. To fix this problem, in the container's parent container, change its size from any fixed size (in the case of her example, `grid-template-rows: 50px 2fr 3fr 50px`) to a flexible size using `auto` (in her example, `grid-template-rows: auto 2fr 3fr 50px`).
