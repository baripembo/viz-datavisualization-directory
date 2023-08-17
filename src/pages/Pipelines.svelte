<script>
  export let data
  
  let sortBy = {col: 'Name', ascending: true};
  
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
</script>

<section>
  <table>
    <thead>
      <tr>
        <th class='sortable' on:click={sort('Name')}>Name <i class='icon-sort'></i></th>
        <th class='sortable' on:click={sort('Status')}>Status <i class='icon-sort'></i></th>
        <th>Owner</th>
        <th>Notes</th>
        <th>Run location</th>
        <th>Schedule</th>
        <th>Duration</th>
      </tr>
    </thead>
    <tbody>
      {#each data as row}
        <tr>
          <td><a href={row['Scraper repository']} target='_blank'>{row['Name']}</a></td>
          <td class={row['Status'].replace(/\s/g, '')}>{row['Status']}</td>
          <td>
            {#if row['Owner email']}
              <a href="mailto:{row['Owner email']}">{row['Owner']}</a>
            {:else}
              {row['Owner']}
            {/if}
          </td>
          <td>{row['Notes']}</td>
          <td>
            {#if row['Run URL']}
              <a href={row['Run URL']} target='_blank'>{row['Run location']}</a>
            {:else}
              {row['Run location']}
            {/if}
          </td>
          <td>{row['Schedule']}</td>
          <td>{row['Duration']}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</section>