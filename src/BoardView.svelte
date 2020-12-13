<script>
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";
  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/storage";

  export let boardId;
  export let ownerId;

  async function loadSounds() {
    const soundsRef = firebase
      .storage()
      .ref(`users/${ownerId}/boards/${boardId}/sounds`);
    return Promise.all(
      (await soundsRef.listAll()).items.map((s) => s.getDownloadURL())
    );
  }
</script>

<h1>I am board id {ownerId}/{boardId}</h1>
<Button on:click={() => navigate(`/b/${boardId}/edit`)}>Edit</Button>
{#await loadSounds() then sounds}
  <Board {sounds} />
{/await}
