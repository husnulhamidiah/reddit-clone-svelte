<script>
  import { createEventDispatcher } from 'svelte'
  import moment from 'moment'
  import { userStore } from './store'

  export let id, comments
  const dispatch = createEventDispatcher()

  let user = {}
  userStore.subscribe(value => {
    if (value) user = value
  })

  const removeComment = async (event) => {
    event.preventDefault()
    const { srcElement: { id: commentId } } = event

    const url = `API_BASE_URL/post/${id}/${commentId}`
    const token = localStorage.getItem('token')

    const res = await fetch(url, {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      }
    })

    if (!res.ok) alert('Something went wrong!')
    const post = await res.json()
    dispatch('update-comment', post)
  }
</script>

<style>
  .content {
    width: 100%;
  }
  .comment-container {
    margin-bottom: 1em;
  }
  .comment-metadata {
    font-size: 12px;
  }
  .comment-body {
    font-size: 14px;
  }
  .remove-button {
    cursor: pointer;
  }
</style>

<div class="content">
  {#each comments as comment}
    <div class="comment-container">
      <div class="comment-metadata">
        <span>{ comment.author.username }</span> · <span>{ moment(comment.created).fromNow() }</span>
        {#if comment.author.id === user.id }
          <span id={ comment.id } class="remove-button float-right" on:click={ removeComment }>Delete</span>
        {/if}
      </div>
      <div class="comment-body">
        <span>{ comment.body }</span>
      </div>
    </div>
  {/each}
</div>
