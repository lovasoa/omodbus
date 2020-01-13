<script>
  import {
    Button,
    FormGroup,
    ListGroup,
    ListGroupItem,
    Input,
    Label,
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardImg,
    CardSubtitle,
    CardText,
    CardTitle
  } from "sveltestrap";
  import Unit from "./Unit.svelte";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let server;
  addUnit();

  function addUnit() {
    server.units = server.units.concat({
      id: 0 | (Math.max(...server.units.map(u => u.id)) + 1),
      registers: []
    });
  }
  function deleteUnit(unit) {
    server.units = server.units.filter(u => u !== unit);
  }
</script>

<Card class="mb-3">
  <CardHeader>
    <CardTitle>
      🤖 {server.name}
      <code>{server.host}:{server.port}</code>
    </CardTitle>
  </CardHeader>
  <CardBody>
    <CardSubtitle>
      <FormGroup>
        <Label>
          Name
          <Input type="text" placeholder="My server" bind:value={server.name} />
        </Label>
        <Label>
          Host
          <Input
            type="text"
            placeholder="127.0.0.1"
            required
            bind:value={server.host} />
        </Label>
        <Label>
          Port
          <Input
            type="text"
            placeholder="5502"
            required
            bind:value={server.port} />
        </Label>
      </FormGroup>
    </CardSubtitle>
    <CardText>
      <ListGroup>
        {#each server.units as unit (unit.id)}
          <ListGroupItem>
            <Unit {unit} on:delete={deleteUnit(unit)} />
          </ListGroupItem>
        {/each}
      </ListGroup>
    </CardText>
  </CardBody>
  <CardFooter>
    <Button on:click={_ => addUnit()}>
      ➕ Add a new unit to
      <i>{server.name}</i>
    </Button>
    <Button on:click={_ => dispatch('delete')}>❌ Delete</Button>
  </CardFooter>
</Card>
