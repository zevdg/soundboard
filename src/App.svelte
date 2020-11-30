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
