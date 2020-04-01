# Flexbox

## Flex Container Properties
### `display: flex`
Enable flexbox in the container

### `flex-direction`
Defines the direction of the main axis
- `row` (default)
- `row-reverse`
- `column`
- `column-reverse`

### `justify-content`
Aligns flex items along the main axis
- `flex-start` (default)
- `flex-end`
- `center`
- `space-between`
- `space-around`

### `align-items`
Aligns flex items along the cross axis
- `flex-start` (default)
- `flex-end`
- `center`
- `space-between`
- `space-around`

### `flex-wrap`
Specifies whether flex items are forced on a single line or can be wrapped on multiple lines
- `nowrap` (default)
- `wrap`
- `wrap-reverse`

### `align-content`
Aligns a flex container's lines within the flex container when there is extra space on the cross axis
- `flex-start`
- `flex-end`
- `center`
- `space-between`
- `space-around`
- `stretch` (default)

### `flex-flow`
Short-hand notation for `flex-direction` and `flex-wrap`
- `<flex-direction> <flex-wrap>`


## Flex Item Properties
### `order`
Specifies the order of the flex item
- `(..., -1, 0, 1, ...)` (default: 0)

### `flex-grow`
Specifies how much a flex item will grow relative to the rest of the flex items
- `(..., -1, 0, 1, ...)` (default: 0)

### `flex-shrink`
Specifies how much a flex item will shrink relative to the rest of the flex items
- `(..., -1, 0, 1, ...)` (default: 1)

### `flex-basis`
Specifies the initial length of a flex item
- `... px` or `... rem`

### `flex`
Short-hand notation for `flex-grow`, `flex-shrink` and `flex-basis`
- `<flex-grow> <flex-shrink> <flex-basis>`

### `align-self`
Aligns a flex item along the cross axis, override the `align-items` value
- `flex-start` (default)
- `flex-end`
- `center`
- `baseline`
- `stretch`


## Links
- [Flexbox Froggy](https://flexboxfroggy.com/)
- [w3schools](https://www.w3schools.com/css/css3_flexbox.asp)
- [Cheatsheet](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)