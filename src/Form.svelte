<script>
  import materialStore from "./material-store";

  export let id;
  export let name = "";
  export let price = 5;

  $: mode = id ? "edit" : "add";
  $: canSubmit = price >= 0 && name !== "";

  function submit() {
    if (!canSubmit) return;

    if (mode === "add") {
      materialStore.add(name, price);
    }

    if (mode === "edit") {
      materialStore.edit(id, name, price);
    }

    price = 5;
    name = "";
    id = undefined;
  }

  function cancel() {
    price = 5;
    name = "";
    id = undefined;
  }
</script>

<style>
  button {
    margin-left: 20px;
  }

  button:disabled {
    cursor: not-allowed;
  }
</style>

<form on:submit|preventDefault={submit}>
  <fieldset>
    <label for="material">Material</label>
    <input
      bind:value={name}
      type="text"
      name="nameField"
      placeholder="Wood, Glue, Etc"
      id="nameField" />
  </fieldset>
  <fieldset>
    <label for="priceField">Material</label>
    <input
      bind:value={price}
      type="number"
      name="priceField"
      placeholder="Price"
      min="0"
      step="any"
      id="nameField" />
  </fieldset>
  <button disabled={!canSubmit} class="float-right" type="submit">
    {mode}
  </button>
  {#if mode === 'edit'}
    <button on:click={cancel} class="float-right" type="button">Cancel</button>
  {/if}
</form>
