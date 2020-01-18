<script>
import { tick, createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

// Props
export let value = '';
export let type = 'text';
export let placeholder = '';
export let labelClasses = '';
export let inputClasses = '';
export let rows = 2;
export let cols = 20;
export let options = [];

let editing = false;
let inputEl;
let label;
let selectedIndex = options.findIndex(o => o.value === value);

// Computed
$: isText = type === 'text';
$: isNumber = type === 'number';
$: isTextArea = type === 'textarea';
$: isSelect = type === 'select';
$: if (isNumber) {
      label = value === '' ? placeholder : value;
    } else if (isText || isTextArea) {
      label = value ? value : placeholder;
    } else { // Select
      label = selectedIndex === -1 ? placeholder : options[selectedIndex].label;
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
  dispatch('blur', value);
};

const handleChange = (e) => {
  selectedIndex = placeholder ? e.target.selectedIndex - 1 : e.target.selectedIndex;
  value = options[selectedIndex].value;
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
{:else if editing && isSelect}
  <select 
    class={inputClasses}
    bind:this={inputEl}
    {value}
    on:blur={handleBlur}>
    {#if placeholder}
      <option selected value disabled>{placeholder}</option>
    {/if}
    {#each options as { label, value }, i}
      <option 
        key={i}
        {value}>
        {label}
      </option>
    {/each}
  </select>
{:else}
  <span 
    class={labelClasses}
    on:click={toggle}>
    {label}
    <slot name="selectCaret">
      {#if isSelect}
        <span>&#9660;</span>
      {/if}
    </slot>
  </span>
{/if}