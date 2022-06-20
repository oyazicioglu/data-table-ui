<script>
    import { Table } from 'data-table';
    import { onMount } from 'svelte';

    const columns = [
        {
            name: 'Name',
            sortable: true,
            visible: true,
            selectable: true,
            type: 'string',
        },
        {
            name: 'Age',
            sortable: true,
            visible: true,
            selectable: true,
            type: 'number',
        },
    ];

    const rows = [
        {
            name: 'John',
            age: '30',
        },
        {
            name: 'Joe',
            age: '20',
        },
        {
            name: 'Jane',
            age: '40',
        },
    ];

    let searchValue = undefined;
    let filteredRows = [];

    const table = new Table();
    table.createFromJSON(columns, rows);
    filteredRows = table.rows;

    const search = (value) => {
        if (!value) {
            filteredRows = table.rows;
            return;
        }

        filteredRows = table.search(value);
    };

    $: search(searchValue);

    onMount(async () => {});
</script>

<main>
    <input type="search" name="search" bind:value={searchValue} />
    <table>
        <thead>
            <tr>
                {#if table.header}
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
