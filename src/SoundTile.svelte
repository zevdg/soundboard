<script>
  import { ClickableTile } from "carbon-components-svelte";
  import Icon from "fa-svelte";
  import { faMicrophoneAlt } from "@fortawesome/free-solid-svg-icons/faMicrophoneAlt";
  import { faPlay } from "@fortawesome/free-solid-svg-icons/faPlay";
  export let idx;
  export let edit;
  export let sound = null;
</script>

<style>
  :global .sound .record-icon {
    font-size: 5em;
    margin: auto;
    height: 100%;
  }
  :global .sound .play-icon {
    font-size: 5em;
    margin: auto;
    height: 100%;
  }
  :global .sound > a {
    flex-grow: 1;
  }
  .sound {
    text-align: center;
    display: flex;
  }
</style>

<div class="sound">
  {#if sound}
    <ClickableTile>
      <Icon class="play-icon" icon={faPlay} />
    </ClickableTile>
  {/if}
  {#if edit}
    <ClickableTile>
      <Icon class="record-icon" icon={faMicrophoneAlt} />
    </ClickableTile>
  {/if}
</div>
