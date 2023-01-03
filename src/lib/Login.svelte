<script lang="ts">
    import {currentUser, pb} from './pocketbase'

    let username: string;
    let password: string;

    async function login() {
        await pb.collection('users').authWithPassword(username, password)
    }

    async function signUp() {
        try {
            await pb.collection('users').create({
                username, 
                password
            })
            login()
        } catch (e) {
            console.error(e)
        }
    }

    function signOut() {
        pb.authStore.clear()
    }
    
</script>

{#if $currentUser}
    <p>Signed in as {$currentUser.username}</p>
    <button on:click={signOut}>Sign Out</button>
{:else}
    <form on:submit|preventDefault>
        <input bind:value={username} type="text" placeholder="Username">
        <input bind:value={password} type="password" placeholder="Password">
        <button on:click={signUp}>Sign Up</button>
        <button on:click={login}>Login</button>
    </form>
{/if}