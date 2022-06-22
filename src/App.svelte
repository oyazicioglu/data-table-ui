<script context="module">
    export async function fetchPosts() {
        return fetch('https://jsonplaceholder.typicode.com/posts')
            .then((response) => response.json())
            .then((posts) => posts);
    }
</script>

<script>
    import 'bulma-svelte/src/styles/main.scss';
    import { onMount } from 'svelte';
    import { columns } from './lib/columnsDefinitions.js';
    import { DataTable } from 'data-table';
    import Table from 'bulma-svelte/src/components/table/table.svelte';
    import TableBody from 'bulma-svelte/src/components/table/table-body.svelte';
    import TableHead from 'bulma-svelte/src/components/table/table-head.svelte';
    import TableCell from 'bulma-svelte/src/components/table/table-cell.svelte';
    import TableCellHeading from 'bulma-svelte/src/components/table/table-cell-heading.svelte';
    import TableContainer from 'bulma-svelte/src/components/table/table-container.svelte';
    import TableRow from 'bulma-svelte/src/components/table/table-row.svelte';
    import Input from 'bulma-svelte/src/components/form/input.svelte';
    import Icon from '../../bulma-svelte/src/components/icon/icon.svelte';
    import Button from '../../bulma-svelte/src/components/button/button.svelte';
    import Buttons from '../../bulma-svelte/src/components/button/buttons.svelte';
    import Block from '../../bulma-svelte/src/components/block/block.svelte';
    import Container from '../../bulma-svelte/src/components/layout/container.svelte';
    import Dropdown from '../../bulma-svelte/src/components/dropdown/dropdown.svelte';
    import DropdownMenu from '../../bulma-svelte/src/components/dropdown/dropdown-menu.svelte';
    import DropdownItem from '../../bulma-svelte/src/components/dropdown/dropdown-item.svelte';
    import Level from '../../bulma-svelte/src/components/level/level.svelte';
    import LevelSide from '../../bulma-svelte/src/components/level/level-side.svelte';
    import LevelItem from '../../bulma-svelte/src/components/level/level-item.svelte';

    let searchValue = undefined;
    /** @type {DataTable}*/
    const table = new DataTable();
    let tableColumns;
    let tableRows;
    let tableHeader;
    let filterInputs = [];

    /**
     * @param {string} value
     */
    const search = (value) => {
        return table.globalSearch(value?.trim().split(' '));
    };

    table.onUpdate((result) => {
        tableRows = result;
        console.log(result);
    });

    table.onColumnsChanged((columns) => {
        tableColumns = columns;
        console.log(columns);
    });

    table.onRowsChanged((rows) => {
        tableRows = rows;
        console.log(rows);
    });

    table.onHeaderChanged((header) => {
        tableHeader = header;
        console.log(header);
    });

    const hideColumn = (column) => {
        console.log(column);
        table.hideColumn(column);
    };

    onMount(async () => {
        const posts = await fetchPosts();

        table.createFromJSON(columns, posts);
        tableRows = table.rows;
        tableColumns = table.columns;
        tableHeader = table.header;
    });
</script>

<main>
    <Container isFluid>
        <Block>
            <Input
                isExpended
                hasAddons
                type="search"
                on:keypress={(e) => {
                    if (e.key === 'Enter') {
                        search(searchValue);
                    }
                }}
                bind:value={searchValue}>
                <Button
                    disabled={!searchValue}
                    color="is-primary"
                    on:click={() => {
                        search(searchValue);
                    }}><Icon iconName="search" /></Button>
            </Input>
        </Block>

        <TableContainer>
            <Table isFullWidth isHoverable isStriped>
                <TableHead>
                    <TableRow>
                        {#if tableHeader}
                            {#each tableHeader?.cells as cell, i}
                                {#if cell.visibility}
                                    <TableCellHeading
                                        ><Level>
                                            <LevelSide>
                                                <LevelItem>{cell.value}</LevelItem>
                                            </LevelSide>
                                            <LevelSide side="level-right">
                                                <LevelItem>
                                                    <Dropdown
                                                        isHoverable
                                                        isRight={i === tableHeader?.cells.length - 1}
                                                        iconName="ellipsis-v"
                                                        text="">
                                                        <DropdownMenu>
                                                            <DropdownItem asLink={false}>
                                                                <Buttons hasAddons>
                                                                    <Button
                                                                        on:click={() => {
                                                                            hideColumn(cell.column);
                                                                        }}><Icon iconName="eye-slash" /></Button>
                                                                    <Button><Icon iconName="sort" /></Button>
                                                                </Buttons>
                                                            </DropdownItem>
                                                            <DropdownItem isDivider />
                                                            <DropdownItem asLink={false}>
                                                                <Input
                                                                    bind:value={filterInputs[i]}
                                                                    leftIconName="filter"
                                                                    style="width:120px"
                                                                    isExpended
                                                                    size="is-small"
                                                                    type="input" />
                                                            </DropdownItem>
                                                        </DropdownMenu>
                                                    </Dropdown>
                                                </LevelItem>
                                            </LevelSide>
                                        </Level>
                                    </TableCellHeading>
                                {/if}
                            {/each}
                        {/if}
                    </TableRow>
                </TableHead>
                <TableBody>
                    {#if tableRows && tableRows.length > 0}
                        {#each tableRows as row}
                            <TableRow>
                                {#each row.cells as cell}
                                    <TableCell>{cell.value}</TableCell>
                                {/each}
                            </TableRow>
                        {/each}
                    {/if}
                </TableBody>
            </Table>
        </TableContainer>
    </Container>
</main>

<style lang="scss">
</style>
