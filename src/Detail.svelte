<script>
  import { onMount } from 'svelte'
  import Post from './Post.svelte'
  import Comment from './Comment.svelte'
  import CommentForm from './CommentForm.svelte'
  import { userStore } from './store';

  export let category = null, postId = null;
  let post = null

  let user;
  const unsubscribe = userStore.subscribe(value => {
    user = value
  });

  const fetchPost = async ({ postId }) => {
    let url = `http://local.host:8080/api/post/${postId}`

    const res = await fetch(url)
    if (!res.ok) return alert('Something wrong!');
    post = await res.json()
  }

  $: fetchPost({ postId })

  const updateComment = (event) => {
		post = event.detail
	}
</script>

{#if post}
  <div class="row">
    <div class="column column-75">
      <Post { post } withDetails={ true }></Post>
      {#if user}
        <CommentForm id={ post.id } on:update-comment={ updateComment }></CommentForm>
      {/if}
      <Comment id={ post.id } comments={ post.comments } on:update-comment={ updateComment }></Comment>
    </div>
  </div>
{/if}