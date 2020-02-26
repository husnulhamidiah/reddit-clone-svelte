<script>
  import Post from './Post.svelte'
  import { userStore, currentCategory } from './store'

  export let username = null
  export let category = null
  let posts = []

  $: {    
    if (category) {
      document.title = `${category} - upvotocracy.com`
      currentCategory.set(category)
    }
    else {
      document.title = 'upvotocracy.com'
    } 
  }

  const fetchPost = async ({ username, category }) => {
    let url = 'API_BASE_URL'

    if (username) url += `/user/${username}`
    else if (category) url += `/posts/${category}`
    else url += '/posts'

    const res = await fetch(url)
    if (!res.ok) return alert('Something wrong!')
    posts = await res.json()
  }

  $: fetchPost({ username, category })

  let user
  userStore.subscribe(value => {
    if (value) {
      user = value
    }
  })

  function sortBy(type = 'hot') {
    posts = posts.sort((a, b) => {
      if (type === 'hot') {
        return b.score - a.score;
      }

      if (type === 'comments') {
        return b.comments.length - a.comments.length;
      }
      
      return new Date(b.created).getTime() - new Date(a.created).getTime();
    });
  }
</script>

<nav>
  <a href="#hot" on:click|preventDefault={() => sortBy('hot')}>Hot</a>
  <a href="#new" on:click|preventDefault={() => sortBy('new')}>New</a>
  <a href="#comments" on:click|preventDefault={() => sortBy('comments')}>Comments</a>
  <a href={`/api/1/posts/${category?category+'/' : ''}rss`}>RSS</a>
</nav>
{#each posts as post}
  <Post { post }></Post>
{/each}
