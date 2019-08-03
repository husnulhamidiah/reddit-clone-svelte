<script>
	import { onMount } from 'svelte';
	import { Link } from "svelte-routing";
	import Post from './Post.svelte';
	import { userStore } from './store';

	export let username = null, category = null;
	let posts = []

	const fetchPost = async ({ username, category }) => {
		let url = 'http://local.host:8080/api'

		if (username) url += `/user/${username}`
		else if (category) url += `/posts/${category}`
		else url += '/posts'

		const res = await fetch(url)
		if (!res.ok) return alert('Something wrong!');
		posts = await res.json()
	}

	$: fetchPost({ username, category });

  let user;
  const unsubscribe = userStore.subscribe(value => {
    if(value) user = value
  });
</script>

<div class="row">
  <div class="column column-75">
    {#each posts as post}
      <Post { post }></Post>
    {/each}
  </div>

	{#if user}
		<div class="column column-25">
			<Link to="/compose"><span class="button">Create a post</span></Link>
		</div>
	{/if}
</div>