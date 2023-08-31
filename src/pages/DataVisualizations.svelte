<script>
  import { onMount } from 'svelte';
  //import { sort } from '../lib/Sort.svelte';
  export let data
  
  let sortBy = {col: 'Date created', ascending: true};
  
  $: sort = (column) => {
    
    if (sortBy.col == column) {
      sortBy.ascending = !sortBy.ascending
    } else {
      sortBy.col = column
      sortBy.ascending = true
    }
    
    // Modifier to sorting function for ascending or descending
    let sortModifier = (sortBy.ascending) ? 1 : -1;
    
    let sort = (a, b) => 
      (a[column] < b[column]) 
      ? -1 * sortModifier 
      : (a[column] > b[column]) 
      ? 1 * sortModifier 
      : 0;
    
      data = data.sort(sort);
  }

  onMount(() => {
    console.log('am i here')
    sort('Date created');
    console.log(data)
  })
</script>

<section>
  <table>
    <thead>
      <tr>
        <th class='sortable' on:click={sort('Name')}>Name <i class='icon-sort'></i></th>
        <th class='sortable' on:click={sort('Status')}>Status <i class='icon-sort'></i></th>
        <th>Owner</th>
        <th>Notes</th>
        <th class='sortable' on:click={sort('Date created')}>Date created <i class='icon-sort'></i></th>
        <th class='sortable' on:click={sort('Date retired')}>Date retired <i class='icon-sort'></i></th>
        <th>Code</th>
        <th>Scraper</th>
        <th>Datasheet</th>
      </tr>
    </thead>
    <tbody>
      {#each data as row}
        <tr>
          <td><a href={row['Link']} target='_blank'>{row['Name']}</a></td>
          <td class={row['Status'].replace(/\s/g, '')}>{row['Status']}</td>
          <td>
            {#if row['Owner email']}
              <a href="mailto:{row['Owner email']}">{row['Owner']}</a>
            {:else}
              {row['Owner']}
            {/if}
          </td>
          <td>{row['Notes']}</td>
          <td>{row['Date created']}</td>
          <td>{row['Date retired']}</td>
          <td>
            {#if row['Code repository']}
              <a href={row['Code repository']} target='_blank'>Code</a>
            {/if}
          </td>
          <td>
            {#if row['Scraper repository']}
              <a href={row['Scraper repository']} target='_blank'>Scraper</a>
            {/if}
          </td>
          <td>
            {#if row['Data sheet']}
              <a href={row['Data sheet']} target='_blank'>Datasheet</a>
            {/if}
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</section>