<script>
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";

  export let id;
</script>

<h1>I am board id {id}</h1>
<p>Press and hold microphone to record.</p>
<Board {id} edit={true} />
<Button on:click={() => navigate(`/b/${id}`)}>Save</Button>
