<script>
  import { Link } from "svelte-routing";
  import { userStore } from './store';

  let user;
  const unsubscribe = userStore.subscribe(value => {
    user = value
  });

  const logout = () => {
    localStorage.removeItem('token')
    userStore.update(() => undefined)
  };

  let categories = ['tech', 'programming', 'dota2', 'underlords', 'gif', 'android'];
</script>


<style>
  .navbar {
    padding-bottom: 5px;
    border-bottom: 1px solid #d1d1d1;
    margin-top: 30px;
  }
  .navbar .float-right .navbar-item { margin-left: 20px; }
  .categories-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 10px 0 5px 0;
    margin-bottom: 30px;
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