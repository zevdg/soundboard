<script>
  import Home from "./Home.svelte";
  import BoardEdit from "./BoardEdit.svelte";
  import BoardView from "./BoardView.svelte";
  import { Router, Route } from "svelte-routing";
  import {
    Content,
    Header,
    HeaderNav,
    HeaderNavItem,
    SkipToContent,
  } from "carbon-components-svelte";
  import firebase from "firebase/app";
  import "firebase/auth";

  const firebaseConfig = {
    apiKey: "AIzaSyAKMPjT87uORMerVlQvrvDy5QaHtJbtZI4",
    authDomain: "mysoundboards.firebaseapp.com",
    projectId: "mysoundboards",
    storageBucket: "mysoundboards.appspot.com",
    messagingSenderId: "1028333641268",
    appId: "1:1028333641268:web:1c5493259aed7c067c7461",
  };

  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);

  export let url = "";
</script>

<style global>
  @import "carbon-components-svelte/css/g100";

  /* 
  Permanently hide hamburger menu 
  https://github.com/IBM/carbon-components-svelte/issues/434 
  */
  header > button:nth-of-type(1) {
    display: none !important;
  }

  #app-content {
    height: 100%;
  }
</style>

<Router {url}>
  <Header platformName="Soundboard">
    <div slot="skip-to-content">
      <SkipToContent />
    </div>
    <HeaderNav>
      <HeaderNavItem href="/" text="Home" />
    </HeaderNav>
  </Header>
  <Content id="app-content">
    <Route path="b/:id/*" let:params>
      <Router>
        <Route path="edit">
          <BoardEdit id={params.id} />
        </Route>
        <Route path="/">
          <BoardView id={params.id} />
        </Route>
      </Router>
    </Route>
    <Route path="/">
      <Home />
    </Route>
  </Content>
</Router>
