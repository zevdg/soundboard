<script>
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";

  export let id;
</script>

<h1>I am board id {id}</h1>
<Button on:click={() => navigate(`/b/${id}/edit`)}>Edit</Button>
<Board {id} edit={false} />
