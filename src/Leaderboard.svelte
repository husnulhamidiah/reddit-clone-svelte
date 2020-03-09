<h1>Leaderboard</h1>

<ol>
{#each leaders as user}
    <li><a href="/u/{user.username}">{user.username}</a> {user.karma}</li>
{/each}
</ol>

<script>

import { onMount } from 'svelte';
import { abbreviateNumber } from './utils/abbreviateNumber';

let leaders = [];

function getLeaderBoard(){
    return fetch('API_BASE_URL/leaderboard')
        .then(async res => {
            if (res.ok) {
                return await res.json();
            } else {
                throw res;
            }
        })
        .catch(console.error);
}

onMount(async () => {
    const res = await getLeaderBoard()
        .catch(console.error);

    leaders = res.leaders;
})
</script>