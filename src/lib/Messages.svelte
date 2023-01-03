<script lang="ts">
    import {onMount, onDestroy} from 'svelte'
    import { pb } from './pocketbase';

    let messages = []
    let unsubscribe;

    onMount(async () => {
        const recentList = await pb.collection('messages').getList(1, 50, {
            sort: 'created',
            expand: 'user'
        })
        
        messages = recentList.items



        unsubscribe = pb.collection('messages').subscribe('*', async ({action, record}) => {
            if(action === 'create') {
                const user = await pb.collection('users').getOne(record.user)
                record.expand = {user}
                messages = [...messages, record]
            }
            if(action === 'delete') {
                messages = messages.filter(m => m.id !== record.id)
            }
        })
    })

    onDestroy(() => {
        unsubscribe?.()
    })
</script>

<div class="messages">
    {#each messages as message (message.id) }
        <div class="message">
            <img class="avatar" src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`} alt="avatar" width="40px">
            <p class="text"><strong>{message.expand?.user?.username}: </strong>{message.text}</p>
        </div>
    {/each}
</div>