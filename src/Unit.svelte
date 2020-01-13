<script>
  import {
    Button,
    Form,
    FormGroup,
    FormText,
    Input,
    Label,
    Table,
    Navbar,
    NavbarBrand
  } from "sveltestrap";
  import Register from "./Register.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let unit;
  addRegister();

  function addRegister() {
    unit.registers = unit.registers.concat({
      name: "",
      address: 0,
      type: "int16",
      value: 0
    });
  }
  function deleteRegister(register) {
    unit.registers = unit.registers.filter(r => r !== register);
  }
</script>

<style>
  .unit-num {
    font-size: 0.8em;
    padding: 1px;
    margin: 0;
    width: 65px;
  }
</style>

<h4>
  Unit
  <input
    class="unit-num"
    type="number"
    min="0"
    max="255"
    bind:value={unit.id} />
  <Button size="sm" title="Delete unit {unit.id}" on:click={_ => addRegister()}>
    ➕ New register
  </Button>
  <Button
    size="sm"
    title="Delete unit {unit.id}"
    on:click={_ => dispatch('delete')}>
    ❌ Delete unit
  </Button>
</h4>

{#if unit.registers.length > 0}
  <Table striped size="sm" responsive>
    <thead>
      <tr>
        <th>Name</th>
        <th>Value</th>
        <th>Address</th>
        <th>Type</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      {#each unit.registers as register (register)}
        <Register {register} on:delete={_ => deleteRegister(register)} />
      {/each}
    </tbody>
  </Table>
{/if}
