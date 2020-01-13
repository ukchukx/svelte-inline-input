<script>
import { tick } from 'svelte';

// Props
export let value = '';
export let type = 'text';
export let placeholder = '';
export let labelClasses = '';
export let inputClasses = '';
export let rows = 2;
export let cols = 20;

let editing = false;
let inputEl;
let label;

// Computed
$: isText = type === 'text';
$: isNumber = type === 'number';
$: isTextArea = type === 'textarea';
$: if (isNumber) {
      label = value === '' ? placeholder : value;
    } else if (isText || isTextArea) {
      label = value ? value : placeholder;
    }

const toggle = async (_) => {
  editing = !editing;

  if (editing) {
    await tick();
    inputEl.focus();
  }
};

const handleInput = (e) => {
  value = isNumber ? +e.target.value : e.target.value;
};

const handleEnter = (e) => {
  if (e.keyCode === 13) inputEl.blur();
};

const handleBlur = (_) => {
  toggle();
};
</script>

{#if editing && (isText || isNumber)}
  <input 
    class={inputClasses}
    bind:this={inputEl}
    {type}
    {value}
    {placeholder}
    on:input={handleInput}
    on:keyup={handleEnter}
    on:blur={handleBlur}>
{:else if editing && isTextArea}
  <textarea
    class={inputClasses}
    bind:this={inputEl}
    {placeholder}
    {value}
    {rows}
    {cols}
    on:input={handleInput}
    on:blur={handleBlur} />
{:else}
  <span 
    class={labelClasses}
    on:click={toggle}>
  {label}
  </span>
{/if}