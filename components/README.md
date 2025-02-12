# **VanUI**: A Collection of Grab 'n Go Reusable UI Components for VanJS

More on **VanJS**:

<div align="center">
  <table>
    <tbody>
      <tr>
        <td>
          <a href="https://github.com/vanjs-org/van/">🏠 Home</a>
        </td>
        <td>
          <a href="https://vanjs.org/start">🖊️ Get Started</a>
        </td>
        <td>
          <a href="https://vanjs.org/tutorial">📖 Tutorial</a>
        </td>
        <td>
          <a href="https://vanjs.org/demo">📚 Examples</a>
        </td>
        <td>
          <a href="https://vanjs.org/convert">📝 HTML to VanJS Converter</a>
        </td>
        <td>
          <a href="https://github.com/vanjs-org/van/discussions">💬 Discuss</a>
        </td>
      </tr>
    </tbody>
  </table>
</div>

🙏 Feedback and contribution are welcome and greatly appreciated!

## Installation

The library is published as NPM package [vanjs-ui](https://www.npmjs.com/package/vanjs-ui).

Run the following command to install the package:

```shell
npm install vanjs-ui
```

To use the NPM package, add this line to your script:

```js
import { <components you want to import> } from "vanjs-ui"
```

## Documentation

The following UI components has been implemented so far:
* [Modal](#modal) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/modal?file=%2Fsrc%2Fmain.ts%3A1%2C1))
* [Tabs](#tabs) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/tabs?file=%2Fsrc%2Fmain.ts%3A1%2C1))
* [MessageBoard](#message) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/message?file=/src/main.ts))
* [Tooltip](#tooltip) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/tooltip?file=/src/main.ts:1,1))
* [Toggle](#toggle) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/toggle?file=%2Fsrc%2Fmain.ts%3A1%2C1))
* [OptionGroup](#optiongroup) ([preview](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/option-group?file=%2Fsrc%2Fmain.ts%3A1%2C1))

### Modal

Creates a modal window on top of the current page.

#### Signature

```js
Modal({...props}, ...children) => <The created modal window>
```

#### Examples

Example 1:

```ts
const closed = van.state(false)
van.add(document.body, Modal({closed},
  p("Hello, World!"),
  div({style: "display: flex; justify-content: center;"},
    button({onclick: () => closed.val = true}, "Ok"),
  ),
))
```

Example 2:

```ts
const closed = van.state(false)
const formDom = form(
  div(input({type: "radio", name: "lang", value: "Zig", checked: true}), "Zig"),
  div(input({type: "radio", name: "lang", value: "Rust"}), "Rust"),
  div(input({type: "radio", name: "lang", value: "Kotlin"}), "Kotlin"),
  div(input({type: "radio", name: "lang", value: "TypeScript"}), "TypeScript"),
  div(input({type: "radio", name: "lang", value: "JavaScript"}), "JavaScript"),
)

const onOk = () => {
  const lang = (<HTMLInputElement>formDom.querySelector("input:checked")).value
  alert(lang + " is a good language 😀")
  closed.val = true
}

van.add(document.body, Modal({closed, blurBackground: true},
  p("What's your favorite programming language?"),
  formDom,
  p({style: "display: flex; justify-content: space-evenly;"},
    button({onclick: onOk}, "Ok"),
    button({onclick: () => closed.val = true}, "Cancel"),
  )
))
```

You can live preview the examples with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/modal?file=%2Fsrc%2Fmain.ts%3A1%2C1).

#### Property Reference

* `closed`: Type `State<boolean>`. Required. A `State` object used to close the created modal window. Basically, setting `closed.val = true` will close the created modal window.
* `backgroundColor`: Type `string`. Default `"rgba(0,0,0,.5)"`. Optional. The color of the background overlay when the modal is activated.
* `blurBackground`: Type `boolean`. Default `false`. Optional. Whether to blur the background.
* `backgroundClass`: Type `string`. Default `""`. Optional. The `class` attribute of the background overlay. You can specify multiple CSS classes seperated by `" "`.
* `backgroundStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the background overlay.
* `modalClass`: Type `string`. Default `""`. Optional. The `class` attribute of the created modal element. You can specify multiple CSS classes seperated by `" "`.
* `modalStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the created modal element.

### Tabs

Creates a tab-view for tabs specified by the user.

#### Signature

```js
Tabs({...props}, tabContents) => <The created tab-view>
```

The `tabContents` parameter is an object whose keys are the titles of the tabs and values (type: `ChildDom | ChildDom[]`) are the DOM element(s) for the tab contents.

#### Example

```ts
van.add(document.body, Tabs(
  {
    style: "max-width: 500px;",
    tabButtonActiveColor: "white",
    tabButtonBorderStyle: "none",
    tabButtonRowStyleOverrides: {
      "padding-left": "12px",
    },
  },
  {
    Home: p(
      "Welcome to ", b("VanJS"), " - the smallest reactive UI framework in the world.",
    ),
    "Getting Started": [
      p("To install the ", b("VanJS"), " NPM package, run the line below:"),
      pre(code("npm install vanjs-core")),
    ],
    About: p(
      "The author of ", b("VanJS"), " is ",
      a({href: "https://github.com/Tao-VanJS"}, " Tao Xin"), "."
    ),
  },
))
```

You can live preview the example with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/tabs?file=%2Fsrc%2Fmain.ts%3A1%2C1).

#### Property Reference

* `activeTab`: Type `State<string> | undefined`. Optional. If specified, you can activate a tab via the specified `State` object with `activeTab.val = "<tab title>"`, and subscribe to the changes of active tab via [`van.derive`](https://vanjs.org/tutorial#api-derive).
* `resultClass`: Type `string`. Default `""`. Optional. The `class` attribute of the result DOM element. You can specify multiple CSS classes seperated by `" "`.
* `style`: Type `string`. Default `""`. Optional. The `style` property of the result DOM element.
* `tabButtonRowColor`: Type `string`. Default `"#f1f1f1"`. Optional. The background color of the container of tab buttons.
* `tabButtonBorderStyle`: Type `string`. Default `1px solid #000`. Optional. The style of borders between tab buttons.
* `tabButtonHoverColor`: Type `string`. Default `"#ddd"`. Optional. The color when the tab button is hovered.
* `tabButtonActiveColor`: Type `string`. Default `"#ccc"`. Optional. The color of the tab button for the currently active tab.
* `transitionSec`: Type `number`. Default `0.3`. Optional. The duration of the transition when tab buttons change color.
* `tabButtonRowClass`: Type `string`. Default `""`. Optional. The `class` attribute of the container of tab buttons. You can specify multiple CSS classes seperated by `" "`.
* `tabButtonRowStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the container of tab buttons.
* `tabButtonClass`: Type `string`. Default `""`. The `class` attribute of tab buttons. You can specify multiple CSS classes seperated by `" "`.
* `tabButtonStyleOverrides`: Type `object`. Default `{}`. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for tab buttons. You can specify multiple CSS classes seperated by `" "`.
* `tabContentClass`: Type `string`. Default `""`. The `class` attribute of tab contents. You can specify multiple CSS classes seperated by `" "`.
* `tabContentStyleOverrides`: Type `object`. Default `{}`. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for tab contents.

### MessageBoard

Creates a message board to show messages on the screen.

#### Signature

To create a message board:

```js
const board = new MessageBoard({...props})
```

Then you can show messages with `show` method:

```js
board.show({...props}) => <The created DOM node for the message, which is also appended to the message board>
```

Optionally, you can remove the DOM node of the message board with `remove` method:

```js
board.remove()
```

#### Examples

```ts
const board = new MessageBoard({top: "20px"})

const example1 = () => board.show({message: "Hi!", durationSec: 1})
const example2 = () => board.show(
  {message: ["Welcome to ", a({href: "https://vanjs.org/", style: "color: #0099FF"}, "🍦VanJS"), "!"], closer: "❌"})

const closed = van.state(false)
const example3 = () => {
  closed.val = false
  board.show({message: "Press ESC to close this message", closed})
}
document.addEventListener("keydown", e => e.key === "Escape" && (closed.val = true))
```

You can live preview the examples with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/message?file=/src/main.ts).

#### Property Reference

Message board properties:

* `top`: Type `string`. Optional. The `top` CSS property of the message board.
* `bottom`: Type `string`. Optional. The `bottom` CSS property of the message board. Exactly one of `top` and `bottom` should be specified.
* `backgroundColor`: Type `string`. Default `"#333D"`. Optional. The background color of the messages shown on the message board.
* `fontColor`: Type `string`. Default `"white"`. Optional. The font color of the messages shown on the message board.
* `fadeOutSec`: Type `number`. Default `0.3`. Optional. The duration of the fade out animation when messages are being closed.
* `boardClass`: Type `string`. Default `""`. Optional. The `class` attribute of the message board. You can specify multiple CSS classes seperated by `" "`.
* `boardStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the message board.
* `messageClass`: Type `string`. Default `""`. Optional. The `class` attribute of the message shown on the message board. You can specify multiple CSS classes seperated by `" "`.
* `messageStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the message shown on the message board.
* `closerClass`: Type `string`. Default `""`. Optional. The `class` attribute of the message closer. You can specify multiple CSS classes seperated by `" "`.
* `closerStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the message closer.

Message properties:

* `message`: Type `ChildDom | readonly ChildDom[]`. Required. One or more `ChildDom` for the message we want to show.
* `closer`: Type `ChildDom | readonly ChildDom[]`. Optional. If specified, we will render a closer DOM node with one or more `ChildDom` which can be clicked to close the shown message.
* `durationSec`: Type `number`. Optional. If specified, the shown message will be automatically closed after `durationSec` seconds.
* `closed`: Type `State<boolean>`. Optional. If specified, the shown message can be closed via the `closed` `State` object with `closed.val = true`.

### Tooltip

Creates a tooltip above a DOM node which typically shows when the DOM node is being hovered.

#### Signature

```js
Tooltip({...props}) => <The created tooltip element>
```

#### Examples

```ts
const tooltip1Show = van.state(false)
const tooltip2Show = van.state(false)
const count = van.state(0)
const tooltip2Text = van.derive(() => `Count: ${count.val}`)
const tooltip3Show = van.state(false)

van.add(document.body,
  button({
    style: "position: relative;",
    onmouseenter: () => tooltip1Show.val = true,
    onmouseleave: () => tooltip1Show.val = false,
  }, "Normal Tooltip", Tooltip({text: "Hi!", show: tooltip1Show})), " ",
  button({
    style: "position: relative;",
    onmouseenter: () => tooltip2Show.val = true,
    onmouseleave: () => tooltip2Show.val = false,
    onclick: () => ++count.val
  }, "Increment Counter", Tooltip({text: tooltip2Text, show: tooltip2Show})), " ",
  button({
    style: "position: relative;",
    onmouseenter: () => tooltip3Show.val = true,
    onmouseleave: () => tooltip3Show.val = false,
  }, "Slow Fade-in", Tooltip({text: "Hi from the sloth!", show: tooltip3Show, fadeInSec: 5})),
)
```

You can live preview the examples with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/tooltip?file=/src/main.ts:1,1).

Note that the lines:

```ts
{
  style: "position: relative;",
  onmouseenter: () => ...Show.val = true,
  onmouseleave: () => ...Show.val = false,
}
```

are needed for the tooltip element to be shown properly.

#### Property Reference

* `text`: Type `string | State<string>`. Required. The text shown in the tooltip. If a `State` object is specified, you can set the text with `text.val = ...`.
* `show`: Type `State<boolean>`. Required. The `State` object to control whether to show the tooltip or not.
* `width`: Type `string`. Default `"200px"`. Optional. The width of the tooltip.
* `backgroundColor`: Type `string`. Default `"#333D"`. Optional. The background color of the tooltip.
* `fontColor`: Type `string`. Default: `"white"`. Optional. The font color of the tooltip.
* `fadeInSec`: Type `number`. Default `0.3`. Optional. The duration of the fade-in animation.
* `tooltipClass`: Type `string`. Default `""`. Optional. The `class` attribute of the tooltip. You can specify multiple CSS classes seperated by `" "`.
* `tooltipStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the tooltip.
* `triangleClass`: Type `string`. Default `""`. Optional. The `class` attribute of the triangle in the bottom. You can specify multiple CSS classes seperated by `" "`.
* `triangleStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the triangle in the bottom.

### Toggle

Creates a toggle switch that can be turned on and off.

#### Signature

```js
Toggle({...props}) => <The created toggle switch>
```

#### Example

```ts
van.add(document.body, Toggle({
  size: 2,
  onColor: "#4CAF50"
}))
```

You can live preview the example with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/toggle?file=%2Fsrc%2Fmain.ts%3A1%2C1).

#### Property Reference

* `on`: Type `boolean | State<boolean>`. Default `false`. Optional. A boolean or a boolean-typed `State` object to indicate the status of the toggle. If a `State` object is specified, you can turn on/off the toggle via the specified `State` object with `on.val = <true|false>`, and subscribe to the status change of the toggle via [`van.derive`](https://vanjs.org/tutorial#api-derive).
* `size`: Type `number`. Default `1`. Optional. The size of the toggle. `1` means the height of the toggle is `1rem`.
* `cursor`: Type `string`. Default `pointer`. Optional. The `cursor` CSS property of the toggle.
* `ofColor`: Type `string`. Default `"#ccc"`. Optional. The color of the toggle when it's off.
* `onColor`: Type `string`. Default `"#2196F3"`. Optional. The color of the toggle when it's on.
* `circleColor`: Type `string`. Default `"white"`. Optional. The color of the toggling circle.
* `toggleClass`: Type `string`. Default `""`. Optional. The `class` attribute of the toggle. You can specify multiple CSS classes seperated by `" "`.
* `toggleStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the toggle.
* `sliderClass`: Type `string`. Default `""`. Optional. The `class` attribute of the slider. You can specify multiple CSS classes seperated by `" "`.
* `sliderStyleOverrides`. Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the slider.
* `circleClass`. Type `string`. Default `""`. Optional. The `class` attribute of the toggling circle. You can specify multiple CSS classes seperated by `" "`.
* `circleStyleOverrides`. Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the toggling circle.
* `circleWhenOnStyleOverrides`. Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the toggling circle. Typically this is used to override the `transform` CSS property if the dimensions of the toggle is overridden.

### OptionGroup

Creates a group of button-shaped options where only one option can be selected. This is functionally similar to a radio group but with a different appearance.

#### Signature

```js
OptionGroup({...props}, options) => <The created option group>
```

The `options` parameter is a `string[]` for all the options.

#### Example

```ts
const selected = van.state("")
const options = ["Water", "Coffee", "Juice"]

van.add(document.body,
  p("What would you like to drink?"),
  OptionGroup({selected}, options),
  p(() => options.includes(selected.val) ?
    span(b("You selected:"), " ", selected) : b("You haven't selected anything.")),
)
```

You can live preview the example with [CodeSandbox](https://codesandbox.io/p/sandbox/github/vanjs-org/van/tree/main/components/examples/option-group?file=%2Fsrc%2Fmain.ts%3A1%2C1).

#### Property Reference

* `selected`: Type `State<string>`. Required. A `State` object for the currently selected option. You can change the selected option with `selected.val = <option string>`, and subscribe to the selection change via [`van.derive`](https://vanjs.org/tutorial#api-derive).
* `normalColor`: Type `string`. Default `"#e2eef7"`. Optional. The color of the option when it's not selected or hovered.
* `hoverColor`: Type `string`. Default `"#c1d4e9"`. Optional. The color of the option when it's hovered.
* `selectedColor`: Type `string`. Default `"#90b6d9"`. Optional. The color of the option when it's selected.
* `selectedHoverColor`: Type `string`. Default `"#7fa5c8"`. Optional. The color of the option when it's selected and hovered.
* `fontColor`: Type `string`. Default `"black"`. Optional. The font color of the options.
* `transitionSec`: Type `number`. Default `0.3`. Optional. The duration of the transition when the options change color.
* `optionGroupClass`: Type `string`. Default `""`. Optional. The `class` attribute of the entire option group. You can specify multiple CSS classes seperated by `" "`.
* `optionGroupStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the entire option group.
* `optionClass`: Type `string`. Default `""`. Optional. The `class` attribute of the options. You can specify multiple CSS classes seperated by `" "`.
* `optionStyleOverrides`: Type `object`. Default `{}`. Optional. A [property bag](#property-bag-for-style-overrides) for the styles you want to override for the options.

### Planned for Future

The following UI components are planned to be added in the future:
* Banner

### Property Bag for Style Overrides

In the API of **VanUI**, you can specify an object as a property bag to override the styles of the created elements. The keys of the property bag are CSS property names, and the values of the property bag are CSS property values. Sample values of the property bag:

```js
{
  "z-index": 1000,
  "background-color": "rgba(0,0,0,.8)",
}
```

```js
{
  "border-radius": "0.2rem",
  padding: "0.8rem",
  "background-color": "yellow",
}
```
