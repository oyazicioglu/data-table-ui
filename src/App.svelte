<script>
    import { Cell, Table } from 'data-table';

    const columns = [
        {
            name: 'Name',
            sortable: true,
            visible: false,
            type: 'string',
            selectable: true,
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

    const table = new Table();
    table.createFromJSON(columns, rows);

    const tableRows = table.rows;
    tableRows.forEach((row) => {
        console.log(row.toValueObject());
    });
</script>

<main>
    <table>
        <thead>
            <tr>
                {#each table.header.cells as cell}
                    <th>{cell.value}</th>
                {/each}
            </tr>
        </thead>

        <tbody>
            {#each table.rows as row}
                <tr>
                    {#each row.cells as cell}
                        <td>{cell.value}</td>
                    {/each}
                </tr>
            {/each}
        </tbody>
    </table>
</main>

<style>
</style>
