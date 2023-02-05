<script>
    import autosize from 'svelte-autosize';
    import { currentUser, userVotes } from "../stores.js";
    import ScoreCounter from "./ScoreCounter.svelte";
    import ReplyButton from "./ReplyButton.svelte";
    import DeleteButton from "./DeleteButton.svelte";
    import DeleteModal from "./DeleteModal.svelte";
    import EditButton from "./EditButton.svelte";
    import AddComment from "./AddComment.svelte";

    export let id;
    export let username;
    export let avatar;
    export let createdAt;
    export let replyingTo = null;
    export let score;
    export let content;
    export let replies;
    export let comments;

    let replying = false;
    let showModal = false;
    let editing = false;

    let currentUserComment = $currentUser.username === username;

    const upVote = () => {
        if ($userVotes.upVoted.includes(id)) {
            return;
        } else if ($userVotes.downVoted.includes(id)) {
            $userVotes.downVoted = $userVotes.downVoted.filter(item => item !== id);
            score += 1;
        } else {
            $userVotes.upVoted = [...$userVotes.upVoted, id];
            score += 1;
        }
    }

    const downVote = () => {
        if ($userVotes.downVoted.includes(id)) {
            return;
        } else if ($userVotes.upVoted.includes(id)) {
            $userVotes.upVoted = $userVotes.upVoted.filter(item => item !== id);
            score -= 1;
        } else {
            $userVotes.downVoted = [...$userVotes.downVoted, id];
            score -= 1;
        }
    }
    
    function deleteComment() {
        comments = comments.filter(comment => comment.id !== id);
        showModal = false;
    }

</script>

<div class="card">
    <div class="score-container">
        <ScoreCounter 
            {score} 
            on:upVote={upVote} 
            on:downVote={downVote}
        />
    </div>
    <img class="avatar" src={avatar} alt="User avatar">
    <span class="username">{username}</span>
    {#if currentUserComment}
        <span class="you-badge">you</span>
    {/if}
    <span class="created-at">{createdAt}</span>
    <div class="button-container">
        {#if currentUserComment}
            <DeleteButton on:click={() => showModal = true}/>
            <EditButton on:click={() => editing=true}/>
        {:else}
            <ReplyButton on:click={() => replying = true}/>
        {/if}
    </div>
    
    {#if editing}
        <div class="edit-container">
            <textarea bind:value={content} placeholder="Add a comment..." use:autosize></textarea>
            <button type="button" on:click={() => editing = false}>Update</button>
        </div>
    {:else}
        <p class="content">
            {#if replyingTo}
                <span class="replying-to">@{replyingTo}</span>
            {/if}
            {content}
        </p>
    {/if}

</div>

{#if showModal}
    <DeleteModal
        on:confirm={deleteComment}
        on:cancel={() => showModal = false}
    />
{/if}

{#if replying}
    <AddComment 
        type="reply"
        replyingTo={username}
        bind:replies
        on:replySubmitted={() => replying = false}
    />
{/if}

{#if replies.length}
    <div class="reply-container">
        {#each replies as reply (reply.id)}
            <svelte:self
                id={reply.id}
                username={reply.user.username}
                avatar={reply.user.image.webp}
                createdAt={reply.createdAt}
                replyingTo={reply.replyingTo}
                bind:score={reply.score}
                bind:content={reply.content}
                bind:replies={reply.replies}
                bind:comments={replies}
            />
        {/each}
    </div>
{/if}

<style>

    .reply-container {
        margin-left: 43px;
        padding-left: 43px;
        border-left: 2px solid var(--light-gray);
        display: flex;
        flex-direction: column;
        row-gap: 24px; 
    }

    .card {
        background: var(--white);
        border-radius: 8px;
        padding: 24px;
        display: grid;
        grid-template-columns: min-content auto auto auto auto 1fr;
        grid-template-rows: auto auto;
        grid-template-areas:
            "score avatar username badge created-at buttons"
            "score content content content content content";
        row-gap: 15px;
    }

    .score-container {
        grid-area: score;
        margin-right: 24px;
    }

    .avatar {
        grid-area: avatar;
        height: 32px;
        width: 32px;
        align-self: center;
    }

    .username {
        grid-area: username;
        font-size: 16px;
        font-weight: 500;
        line-height: 24px;
        color: var(--dark-blue);
        align-self: center;
        margin-left: 16px;
    }

    .you-badge {
        grid-area: badge;
        align-self: center;
        height: 19px;
        width: 36px;
        border-radius: 2px;
        background: var(--moderate-blue);
        color: var(--white);
        padding: 0 6px;
        padding-bottom: 2px;
        font-size: 13px;
        font-weight: 500;
        display: flex;
        align-items: center;
        margin-left: 8px;
    }

    .created-at {
        grid-area: created-at;
        font-size: 16px;
        font-weight: 400;
        line-height: 24px;
        color: var(--grayish-blue);
        align-self: center;
        margin-left: 16px;
    }

    .button-container {
        grid-area: buttons;
        display: flex;
        column-gap: 24px;
        justify-self: end;
    }

    .edit-container {
        grid-area: content;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        gap: 16px;
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

    .content {
        grid-area: content;
        font-size: 16px;
        font-weight: 400;
        line-height: 24px;
        color: var(--grayish-blue);
        white-space: pre-wrap;
    }

    .replying-to {
        font-size: 16px;
        font-weight: 500;
        line-height: 24px;
        color: var(--moderate-blue);
    }

    @media (max-width: 700px) {

        .reply-container {
            margin-left: 16px;
            padding-left: 16px;
            row-gap: 16px; 
        }

        .card {
            padding: 16px;
            grid-template-columns: auto auto auto 1fr;
            grid-template-rows: auto auto auto;
            grid-template-areas:
                "avatar username badge created-at"
                "content content content content"
                "score score buttons buttons";
            row-gap: 16px;
        }

        .score-container {
            margin-right: none;
            justify-self: flex-start;
        }

        .button-container {
            column-gap: 16px;
        }

    }


</style>