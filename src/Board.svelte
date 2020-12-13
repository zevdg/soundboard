<script>
  import SoundTile from "./SoundTile.svelte";
  import { createEventDispatcher } from "svelte";

  export let sounds;
  export let getRecorder;

  const dispatch = createEventDispatcher();

  function indexedGetRecorder(idx) {
    if (getRecorder) {
      return () => {
        return getRecorder(idx);
      };
    }
  }
</script>

<style>
  .board {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 1em;
    min-height: 500px;
    padding-top: 1em;
    padding-bottom: 1em;
  }
</style>

<div class="board">
  {#each sounds as sound, idx}
    <SoundTile
      {sound}
      getRecorder={indexedGetRecorder(idx)}
      on:clear={() => dispatch('clear', { idx })} />
  {/each}
</div>
