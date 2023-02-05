<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<div class="background">
    <div class="card">
        <h2>Delete comment</h2>
        <p>Are you sure you want to delete this comment? This will remove the comment and can’t be undone.</p>
        <div class="button-container">
            <button class="cancel" type=button on:click={() => dispatch('cancel')}>No, cancel</button>
            <button class="delete" type=button on:click={() => dispatch('confirm')}>Yes, delete</button>
        </div>
    </div>
</div>

<style>

    .background {
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background-color: hsla(0, 0%, 0%, 0.5);
        display: grid;
        place-items: center;
    }

    .card {
        width: 400px;
        padding: 32px;
        background: var(--white);
        border-radius: 8px;
        overflow: auto;
    }

    h2 {
        font-size: 24px;
        font-weight: 500;
        color: var(--dark-blue);
    }

    p {
        font-size: 16px;
        font-weight: 400;
        line-height: 24px;
        color: var(--grayish-blue);
        margin: 20px 0;
    }

    .button-container {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 14px;
    }
    
    button {
        height: 48px;
        width: 100%;
        border-radius: 8px;
        text-transform: uppercase;
        color: var(--white);
        cursor: pointer;
    }
    
    .cancel {
        background: var(--grayish-blue);
    }

    .cancel:hover {
        background: var(--light-gray);
        color: var(--grayish-blue);
    }
    
    .delete {
        background: var(--soft-red);
    }

    .delete:hover {
        background: var(--pale-red);
        color: rgb(126, 5, 5);
    }

    @media (max-width: 580px) {

        .card {
            width: 343px;
            padding: 24px 28px;
        }

    }

</style>