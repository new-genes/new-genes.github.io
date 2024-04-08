<!-- components/Search.svelte -->
<script>
  import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell, TableSearch } from 'flowbite-svelte';
  let searchTerm = '';

  let items = [
  { number: 1, maker: 'SJALL014946_D1' },
  { number: 2, maker: 'SJALL014947_D1'},
  { number: 3, maker: 'SJALL014949_D1'},
  { number: 4, maker: 'SJALL014950_D1'},
  { number: 4, maker: 'SJALL014952_D1'}
];
$: filteredItems = items.filter((item) => item.maker.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1);



  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  
  
  let query = '';

  function handleSubmit(event) {
    event.preventDefault();
    dispatch('search', query);
  }
</script>

<form class="justify-center flex w-full" on:submit={handleSubmit}>
  <!--<input type="text" bind:value={query} placeholder="Search..." />
  <button type="submit">검색</button>-->
  <TableSearch placeholder="Search by Patient ID" hoverable={true} bind:inputValue={searchTerm}>
    <TableHead>
      <TableHeadCell>Number</TableHeadCell>
      <TableHeadCell>Patient ID</TableHeadCell>
    </TableHead>
    <TableBody class="divide-y">
      {#each filteredItems as item}
        <TableBodyRow>
          <TableBodyCell>{item.number}</TableBodyCell>
          <TableBodyCell>{item.maker}</TableBodyCell>
        </TableBodyRow>
      {/each}
    </TableBody>
  </TableSearch>
</form>
