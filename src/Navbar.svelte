<script>
  import { Link } from 'svelte-routing'
  import { userStore } from './store'

  let user
  userStore.subscribe(value => {
    user = value
  })

  const logout = () => {
    localStorage.removeItem('token')
    userStore.update(() => undefined)
  }

  const categories = ['tech', 'programming', 'dota2', 'underlords', 'gif', 'android']
</script>

<style>
  .navbar {
    padding-bottom: .4em;
    border-bottom: .1em solid #d1d1d1;
    margin-top: 2em;
  }
  .navbar .float-right .navbar-item { margin-left: 1.2em; }
  .categories-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: .6em 0 .3em 0;
    margin-bottom: 2em;
  }
</style>

<div class="navbar">
  <span><Link to="/"><strong>REDDIT CLONE</strong></Link></span>
  <div class="float-right">
    {#if user}
      <span class="navbar-item"><Link to="/u/{ user.username }">{ user.username.toUpperCase() }</Link></span>
      <span class="navbar-item"><Link on:click={ logout }>LOGOUT</Link></span>
    {:else}
      <span class="navbar-item"><Link to="/login">LOGIN</Link></span>
      <span class="navbar-item"><Link to="/register">REGISTER</Link></span>
    {/if}
  </div>
</div>

<div class="categories-container">
  {#each categories as category}
    <Link to="/a/{ category }"><span>{ category }</span></Link>
  {/each}
</div>