<script>
  import { ClickableTile } from "carbon-components-svelte";
  import Icon from "fa-svelte";
  import { faMicrophoneAlt } from "@fortawesome/free-solid-svg-icons/faMicrophoneAlt";
  import { faPlay } from "@fortawesome/free-solid-svg-icons/faPlay";
  import { faTimes } from "@fortawesome/free-solid-svg-icons/faTimes";
  export let getRecorder;
  export let sound;
  let recording = false;

  async function recordStart(event) {
    event.preventDefault(); // prevent double-firing mouse and touch event

    recording = true;
    let recorder = await getRecorder();
    recorder.start();
  }

  async function recordStop(event) {
    event.preventDefault(); // prevent double-firing mouse and touch event

    let recorder = await getRecorder();
    sound = await recorder.stop();
    recording = false;
  }
</script>

<style>
  :global .SoundTile .icon {
    font-size: 5em;
    margin: auto;
  }
  :global .SoundTile .tileWrapper > a {
    height: 100%;
    display: flex;
  }
  .SoundTile {
    text-align: center;
    display: flex;
  }
  .tileWrapper {
    flex-grow: 1;
  }
</style>

<div class="SoundTile">
  {#if sound}
    <div class="tileWrapper" on:click={sound.play}>
      <ClickableTile>
        <Icon class="icon" icon={faPlay} />
      </ClickableTile>
    </div>
  {/if}
  {#if getRecorder}
    {#if sound}
      <div class="tileWrapper" on:click={() => (sound = null)}>
        <ClickableTile>
          <Icon class="icon" icon={faTimes} />
        </ClickableTile>
      </div>
    {:else}
      <div
        class="tileWrapper"
        on:touchstart={recordStart}
        on:mousedown={recordStart}
        on:touchend={recordStop}
        on:mouseup={recordStop}>
        <ClickableTile clicked={recording}>
          <Icon class="icon" icon={faMicrophoneAlt} />
        </ClickableTile>
      </div>
    {/if}
  {/if}
</div>
