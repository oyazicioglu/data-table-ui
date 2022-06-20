<script context="module">
    export async function fetchPosts() {
        return fetch('https://jsonplaceholder.typicode.com/posts')
            .then((response) => response.json())
            .then((posts) => posts);
    }
</script>

<script>
    import { Table } from 'data-table';
    import { onMount } from 'svelte';
    import { columns } from './lib/columnsDefinitions.js';

    let searchValue = undefined;
    let filteredRows = [];
    let table;

    /**
     * @param {string} value
     */
    const search = (value) => {
        if (!value) {
            if (table?.rows) {
                return table.rows;
            } else {
                return [];
            }
        } else {
            return table.globalSearch(value);
        }
    };

    $: filteredRows = search(searchValue);

    onMount(async () => {
        table = new Table();
        const posts = await fetchPosts();
        table.createFromJSON(columns, posts);
        filteredRows = table.rows;
    });
</script>

<main>
    <input type="search" name="search" bind:value={searchValue} />
    <table>
        <thead>
            <tr>
                {#if table?.header}
                    {#each table.header?.cells as cell}
                        <th>{cell.value}</th>
                    {/each}
                {/if}
            </tr>
        </thead>

        <tbody>
            {#if filteredRows && filteredRows.length > 0}
                {#each filteredRows as row}
                    <tr>
                        {#each row.cells as cell}
                            <td>{cell.value}</td>
                        {/each}
                    </tr>
                {/each}
            {/if}
        </tbody>
    </table>
</main>

<style>
</style>
