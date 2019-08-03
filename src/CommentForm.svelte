<script>
  import { createEventDispatcher } from 'svelte';

  export let id;
  const dispatch = createEventDispatcher();

  const createComment = async (event) => {
    event.preventDefault()
    const form = document.getElementById('comment');
    const formData = new FormData(form);
    form.reset();

    const url = `http://local.host:8080/api/post/${id}`;
    const token = localStorage.getItem('token')

    const res = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        comment: formData.get('comment')
      }),
    })

    if (!res.ok) alert('Something went wrong!');
    const post = await res.json();
    dispatch('update-comment', post);
  }
</script>

<style>
  .content {
    display: flex;
  }
  .content .post-container {
    width: 100%;
    margin-left: 55px;
  }
  .comment-form {
    margin-top: 10px;
  }
  .comment-form .comment-textarea {
    resize: vertical;
    margin-bottom: 0;
  }
</style>

<div class="content">
  <div class="post-container">
    <form id="comment" class="comment-form">
      <fieldset>
        <textarea class="comment-textarea" placeholder="Enter your comment" id="comment" name="comment"></textarea>
        <button class="button button-outline float-right" type="submit" on:click={ createComment }>Comment</button>
      </fieldset>
    </form>
  </div>
</div>
