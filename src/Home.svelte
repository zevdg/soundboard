<script>
  import { Button } from "carbon-components-svelte";
  import { v4 as uuidv4 } from "uuid";
  import { navigate } from "svelte-routing";

  async function createBoard() {
    navigate(`/b/${uuidv4()}/edit`);
  }
</script>

<Button on:click={createBoard}>Create Sound Board</Button>
