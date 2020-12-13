<script>
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";

  async function createBoard() {
    navigate(`/create`);
  }
</script>

<Button on:click={createBoard}>Create Sound Board</Button>
