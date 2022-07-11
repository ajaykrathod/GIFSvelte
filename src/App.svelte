<script>
  import logo from './assets/svelte.png'
  import Giphyjs from './lib/giphyjs.svelte'
  import {Router,Route} from 'svelte-navigator'
  import Header from './lib/Header.svelte'
  import Search from './routes/Search.svelte'
  import Login from './routes/login.svelte';
  import Signup from './routes/signup.svelte';
  import Confirm from './routes/confirm.svelte';
  import Forgot from './routes/forgot.svelte';
  import Reset from './routes/reset.svelte';
</script>

<Router>
  <Header/>
  <main>
    <Route path="/">
      <Giphyjs/>
    </Route>

    <Route path="/search">
      <Search/>
    </Route>

    <Route path="/login">
      <Login/>
    </Route>

    <Route path="/signup">
      <Signup/>
    </Route>

    <Route path="/forgot">
      <Forgot/>
    </Route>

    <Route path="/reset">
      <Reset/>
    </Route>

    <Route path="/confirm">
      <Confirm/>
    </Route>

  </main>
</Router>
<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  main {
    position: relative;
    top: 8vh;
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

</style>
