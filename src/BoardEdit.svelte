<script>
  import { recordAudio } from "./record";
  import Board from "./Board.svelte";
  import { Button } from "carbon-components-svelte";
  import { navigate } from "svelte-routing";
  import { v4 as uuidv4 } from "uuid";
  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/storage";

  firebase.auth().onAuthStateChanged(() => {}); // check for login

  async function signIn() {
    const result = await firebase
      .auth()
      .signInWithPopup(new firebase.auth.GoogleAuthProvider());
    return result.user;
  }

  async function getCurrentUser() {
    return firebase.auth().currentUser || (await signIn());
  }
  const boardId = uuidv4();

  const sounds = new Array(9);
  const recorders = new Array(9);

  async function getRecorder(idx) {
    if (!recorders[idx]) {
      recorders[idx] = await recordAudio(({ blob, mediaRecorder }) => {
        sounds[idx] = {
          blob,
          url: URL.createObjectURL(blob),
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
      .ref(`/users/${user.uid}/boards/${boardId}/sounds`);
    await Promise.allSettled(
      sounds.filter(Boolean).map((sound, idx) =>
        soundsRef.child(idx.toString()).put(sound.blob, {
          contentType: sound.mimeType,
        })
      )
    );
    navigate(`/b/${user.uid}/${boardId}`);
  }

  function clearSound(event) {
    sounds[event.detail.idx] = null;
    recorders[event.detail.idx] = null;
  }
</script>

<p>Press and hold to record.</p>
<Board
  audioSrcs={sounds.map((s) => s?.url)}
  {getRecorder}
  on:clear={clearSound} />
<Button on:click={saveBoard}>Save</Button>
