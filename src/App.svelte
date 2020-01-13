<script>
  import {
    Button,
    FormGroup,
    FormText,
    Input,
    Label,
    Navbar,
    NavbarBrand
  } from "sveltestrap";
  import Server from "./Server.svelte";

  let downloadLink;
  const config = { read: [] };
  addServer();

  function addServer() {
    config.read = config.read.concat({
      name: `Server ${config.read.length}`,
      host: "localhost",
      port: 5502,
      units: []
    });
  }
  function deleteServer(server) {
    config.read = config.read.filter(s => s !== server);
  }

  function download() {
    const json = JSON.stringify(config, null, 2);
    downloadLink.href = `data:text/plain,${encodeURIComponent(json)}`;
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" />
</svelte:head>

<Navbar color="light" light expand="md">
  <NavbarBrand href="/">OModbus</NavbarBrand>
</Navbar>

<main class="container">
  <h2>Servers</h2>
  {#each config.read as server}
    <Server {server} on:delete={deleteServer(server)} />
  {/each}
  <Button on:click={_ => addServer()}>➕ Add a new server</Button>
  <a
    class="btn btn-secondary"
    href="#!"
    download="omodbus.json"
    bind:this={downloadLink}
    on:click={_ => download()}>
    🔽 Download current configuration
  </a>
</main>
