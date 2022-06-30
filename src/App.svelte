<script context="module">
    export async function fetchPosts() {
        return fetch('https://jsonplaceholder.typicode.com/posts')
            .then((response) => response.json())
            .then((posts) => posts);
    }
</script>

<script>
    import 'carbon-components-svelte/css/all.css';
    import { onMount } from 'svelte';
    import DataTable from 'carbon-components-svelte/src/DataTable/DataTable.svelte';
    import Button from 'carbon-components-svelte/src/Button/Button.svelte';
    import ToolbarContent from 'carbon-components-svelte/src/DataTable/ToolbarContent.svelte';
    import ToolbarSearch from 'carbon-components-svelte/src/DataTable/ToolbarSearch.svelte';
    import ToolbarMenu from 'carbon-components-svelte/src/DataTable/ToolbarMenu.svelte';
    import ToolbarMenuItem from 'carbon-components-svelte/src/DataTable/ToolbarMenuItem.svelte';
    import Toolbar from 'carbon-components-svelte/src/DataTable/Toolbar.svelte';
    import { DataTableLogic } from 'data-table';

    let searchValue = undefined;
    let table = new DataTableLogic();

    const headers = [
        { key: 'id', value: 'Id' },
        { key: 'userId', value: 'User Id' },
        { key: 'body', value: 'Body' },
        { key: 'title', value: 'Title' },
    ];

    onMount(async () => {
        table.headers = headers;
        table.rows = await fetchPosts();
    });
</script>

<main>
    <DataTable
        sortable
        title="Load balancers"
        description="Your organization's active load balancers."
        headers={table.headers}
        rows={table.rows}>
        <Toolbar>
            <ToolbarContent>
                <ToolbarSearch bind:value={searchValue} shouldFilterRows />
                <ToolbarMenu>
                    <ToolbarMenuItem primaryFocus>Restart all</ToolbarMenuItem>
                    <ToolbarMenuItem href="https://cloud.ibm.com/docs/loadbalancer-service">API documentation</ToolbarMenuItem>
                    <ToolbarMenuItem hasDivider danger>Stop all</ToolbarMenuItem>
                </ToolbarMenu>
                <Button>Create balancer</Button>
            </ToolbarContent>
        </Toolbar>
    </DataTable>
</main>

<style lang="scss">
</style>
