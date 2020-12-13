<script>
  import { recordAudio } from "./record";
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";
  import { v4 as uuidv4 } from "uuid";
  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/storage";

  async function signIn() {
    const result = await firebase
      .auth()
      .signInWithPopup(new firebase.auth.GoogleAuthProvider());
    return result.user;
  }

  async function getCurrentUser() {
    return firebase.auth().currentUser || (await signIn());
  }
  export let id;

  let sounds = new Array(9);
  let recorders = new Array(9);

  async function getRecorder(idx) {
    if (!recorders[idx]) {
      recorders[idx] = await recordAudio(({ blob, mediaRecorder }) => {
        sounds[idx] = {
          blob,
          mimeType: mediaRecorder.mimeType,
        };
      });
    }
    return recorders[idx];
  }

  async function saveBoard() {
    const user = await getCurrentUser();
    const soundsRef = firebase
      .storage()
      .ref(`/users/${user.uid}/boards/${id}/sounds`);
    for (const recoreder of recorders) {
      if (recorder) {
        soundsRef
          .child(uuidv4())
          .put(recorder.audioBlob, { contentType: "image/jpeg" });
      }
    }
  }

  function clearSound(event) {
    sounds[event.detail.idx] = null;
    recorders[event.detail.idx] = null;
  }
</script>

<p>Press and hold to record.</p>
<Board {id} {sounds} {getRecorder} on:clear={clearSound} />
<Button on:click={() => navigate(`/b/${id}`)}>Save</Button>
