<script>
  import SoundTile from "./SoundTile.svelte";
  import { createEventDispatcher } from "svelte";

  export let id;
  export let sounds;
  export let getRecorder;

  const dispatch = createEventDispatcher();

  // TODO record on button press and hold
  const sleep = (time) => new Promise((resolve) => setTimeout(resolve, time));
  const recordStart = async () => {
    const recorder = await recordAudio();
    const actionButton = document.getElementById("action");
    actionButton.disabled = true;
    recorder.start();
    await sleep(3000);
    const audio = await recorder.stop();
    audio.play();
    await sleep(3000);
    actionButton.disabled = false;
  };
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
      sound={sounds[idx]}
      getRecorder={indexedGetRecorder(idx)}
      on:clear={() => dispatch('clear', { idx })} />
  {/each}
</div>
