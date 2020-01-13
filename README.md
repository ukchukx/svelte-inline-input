# svelte-inline-input

> An inline editable input component for Svelte.

Displays as text and becomes editable by clicking or tapping.

## Installation

```js
npm install svelte-inline-input
```

### Browser

```html
<script type="text/javascript" src="https://unpkg.com/svelte-inline-input"></script>
```

### Module

```js
import InlineInput from 'svelte-inline-input';
```

## Usage

Once installed, it can be used in a template as:

```html
<InlineInput bind:value />
```

See the props table below for the available options.

### Props

| Property | Type | Description | Default |
|:--|:--|:--|:--|
| type | string | The input type. Could be text, number, textarea or select | text |
| placeholder | string | Text to be shown as a placeholder while there is no input |  empty string |
| labelClasses | string | CSS classes for the label element | empty string |
| inputClasses | string | CSS classes for the input element | empty string |
| rows | integer | Textarea rows | 2 |
| cols | integer | Textarea columns | 20 |
| options | array | Provides the options for selects. Each object should have the format `{label: x, value: x}` | [] |

## License

[MIT](http://opensource.org/licenses/MIT)