<script>
  import { Button, Input } from "sveltestrap";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let register;
  const TYPES = ["int16", "coil"];
</script>

<tr>
  <th>
    <Input
      bind:value={register.name}
      placeholder="Register {register.address}" />
  </th>
  <th>
    <Input type="number" bind:value={register.value} />
  </th>
  <th>
    <Input type="number" bind:value={register.address} />
  </th>
  <th>
    <Input
      type="select"
      on:change={e => {
        register.type = e.target.value;
      }}>
      {#each TYPES as typename}
        <option>{typename}</option>
      {/each}
    </Input>
  </th>
  <th>
    <Button
      size="sm"
      title="Delete register"
      on:click={_ => dispatch('delete')}>
      ❌
    </Button>
  </th>
</tr>
