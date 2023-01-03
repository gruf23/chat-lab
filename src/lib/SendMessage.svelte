<script lang="ts">
    import { pb, currentUser } from "./pocketbase";

    let message = ''
    async function send() {
        try {
            await pb.collection('messages').create({
                user: $currentUser.id,
                text: message
            })
            message = ''
        } catch (e) {
            console.error('Cannot send such a message')
        }
    }
    
</script>

<div class="send">
    <form on:submit|preventDefault={send}>
        <input bind:value={message} type="text">
        <button>SEND</button>
    </form>
</div>