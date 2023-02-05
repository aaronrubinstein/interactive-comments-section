<script>
    import { currentUser, comments } from "../stores.js";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let type;
    export let replyingTo = null;
    export let replies = [];

    let content = '';

    function postComment() {
        
        const newComment = {
            id: Math.floor(Math.random() * 100) + 5,
            content: content,
            createdAt: 'Now',
            score: 0,
            replyingTo: replyingTo,
            user: {
                image: {
                    png: $currentUser.image.png,
                    webp: $currentUser.image.webp
                },
                username: $currentUser.username
            },
            replies: []
        }

        if(type === 'new') {
            comments.update(data => [...data, newComment]);
            content = '';
        } else if (type === 'reply') {
            replies = [...replies, newComment];
            dispatch('replySubmitted');
        }
    }

</script>

<div class="card">
    <img class="avatar" src={$currentUser.image.webp} alt="User avatar">
    <textarea bind:value={content} placeholder="Add a comment..."></textarea>
    <button on:click={postComment} type="button">
        {#if type === "new"}
            Send
        {:else}
            Reply
        {/if}
    </button>
</div>

<style>

    .card {
        background: var(--white);
        border-radius: 8px;
        padding: 24px;
        display: grid;
        grid-template-columns: auto 1fr auto;
        gap: 16px;
    }

    .avatar {
        height: 40px;
        width: 40px;
    }

    textarea {
        min-height: 96px;
        width: 100%;
        border: 1px solid var(--light-gray);
        border-radius: 8px;
        font-size: 16px;
        font-weight: 400;
        line-height: 24px;
        color: var(--dark-blue);
        padding: 12px 24px;
        outline: none;
        resize: none;
    }

    textarea::placeholder {
        color: var(--grayish-blue);
    }

    textarea:focus {
        border: 1px solid var(--moderate-blue);
    }

    button {
       height: 48px;
       width: 104px;
       border-radius: 8px;
       background: var(--moderate-blue);
       color: var(--white);
       text-transform: uppercase;
       font-size: 16px;
       font-weight: 500;
       cursor: pointer;
    }

    button:hover {
        background: var(--light-grayish-blue);
    }

    @media (max-width: 580px) {

        .card {
            padding: 16px;
            grid-template-columns: auto auto;
            grid-template-rows: auto auto;
            grid-template-areas: 
                "textarea textarea"
                "avatar button";
        }

        .avatar {
            grid-area: avatar;
            height: 32px;
            width: 32px;
            align-self: center;
        }

        textarea {
            grid-area: textarea;
        }

        button {
            grid-area: button;
            margin-left: auto;
        }

    }

</style>