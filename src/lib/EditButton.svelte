<button type="button" on:click>
    <svg class="icon" width="14" height="14" xmlns="http://www.w3.org/2000/svg"><path d="M13.479 2.872 11.08.474a1.75 1.75 0 0 0-2.327-.06L.879 8.287a1.75 1.75 0 0 0-.5 1.06l-.375 3.648a.875.875 0 0 0 .875.954h.078l3.65-.333c.399-.04.773-.216 1.058-.499l7.875-7.875a1.68 1.68 0 0 0-.061-2.371Zm-2.975 2.923L8.159 3.449 9.865 1.7l2.389 2.39-1.75 1.706Z"/></svg>
    Edit
</button>

<style>

    svg {
            pointer-events: none;
        }    

    button {
        font-size: 16px;
        font-weight: 500;
        line-height: 24px;
        display: flex;
        align-items: center;
        column-gap: 8px;
        cursor: pointer;
        color: var(--moderate-blue);
    }

    .icon {
        fill: var(--moderate-blue);
    }

    button:hover {
        color: var(--light-grayish-blue);
    }

    button:hover > .icon {
        fill: var(--light-grayish-blue);
    }

</style>