<script>
  import { onMount } from 'svelte';
  import { navigate } from "svelte-routing";
  import { userStore } from './store';
  import decode from 'jwt-decode';

  onMount(() => {
    if($userStore) navigate('/')
  })

  const register = async (event) => {
    event.preventDefault();
    const form = document.getElementById('register');
    const formData = new FormData(form);
    form.reset();

    const url = 'http://local.host:8080/api/register';
    const res = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        username: formData.get('username'),
        password: formData.get('password')
      }),
    })

    if (!res.ok) alert('Something went wrong!');
    const { token } = await res.json();

    try {
      userStore.update(() => decode(token).user);
      localStorage.setItem('token', token);
    } catch (error) {
      console.log(error);
    }

    return navigate('/');
  }
</script>

<div class="row">
  <div class="column column-33">
    <form id="register">
      <fieldset>
        <input type="text" placeholder="Username" id="username" name="username">
        <input type="text" placeholder="Password" id="password" name="password">
        <input type="text" placeholder="Confirm Password" id="confirmPassword" name="confirmPassword">

        <button class="button-primary float-right" type="submit" on:click={ register }>Signup</button>
      </fieldset>
    </form>
  </div>
</div>