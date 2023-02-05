<script>
    import { comments } from "./stores.js";
    import Comment from "./lib/Comment.svelte";
    import AddComment from "./lib/AddComment.svelte";
</script>

<main>
    {#each $comments as comment (comment.id)}
        <Comment
            id={comment.id}
            username={comment.user.username}
            avatar={comment.user.image.webp}
            createdAt={comment.createdAt}
            replyingTo={comment.replyingTo}
            bind:score={comment.score}
            bind:content={comment.content}
            bind:replies={comment.replies}
            bind:comments={$comments}
        />
    {/each}
    <AddComment type="new"/>
</main>

<style>

    main {
        max-width: 762px;
        padding: 0 16px;
        width: 100%;
        margin: 64px 0;
        display: flex;
        flex-direction: column;
        row-gap: 20px;
    }

    @media (max-width: 700px) {
        main {
            margin: 32px 0;
        }
    }

</style>
