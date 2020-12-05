<script>
  import { recordAudio } from "./record";
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";

  export let id;

  let sounds = new Array(9);
  let recorders = new Array(9);

  async function getRecorder(idx) {
    if (!recorders[idx] || recorders[idx].getState() == "inactive") {
      recorders[idx] = await recordAudio();
    }
    return recorders[idx];
  }
</script>

<p>Press and hold to record.</p>
<Board {id} {sounds} {getRecorder} />
<Button on:click={() => navigate(`/b/${id}`)}>Save</Button>
