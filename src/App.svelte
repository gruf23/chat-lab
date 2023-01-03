<script>
    import Login from './lib/Login.svelte'
    import Messages from './lib/Messages.svelte';
    import SendMessage from './lib/SendMessage.svelte';
    import { currentUser } from './lib/pocketbase';
</script>

<h1>Chat Lab</h1>
<Login />
{#if currentUser}
    <Messages />
    <SendMessage />
{/if}