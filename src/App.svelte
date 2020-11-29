<script>
  import Home from "./Home.svelte";
  import Board from "./Board.svelte";
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

  /* permanently hide hamburger menu */
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
    <Route path="b/:id" component={Board} />
    <Route path="/" component={Home} />
  </Content>
</Router>
