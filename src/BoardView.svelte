<script>
  import Board from "./Board.svelte";
  import { Loading } from "carbon-components-svelte";
  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/storage";

  export let boardId;
  export let ownerId;

  async function getAudioSrcs() {
    const soundsRef = firebase
      .storage()
      .ref(`users/${ownerId}/boards/${boardId}/sounds`);
    return Promise.all(
      (await soundsRef.listAll()).items.map((s) => s.getDownloadURL())
    );
  }
</script>

<!-- <Button on:click={() => navigate(`/b/${boardId}/edit`)}>Edit</Button> -->
{#await getAudioSrcs()}
  <Loading />
{:then audioSrcs}
  <Board {audioSrcs} />
{/await}
