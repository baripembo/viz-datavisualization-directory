<script>
  import { sort } from '../lib/Sort.svelte';
  export let data;
  
  let sortBy = {col: 'Date created', ascending: false};
  
  $: sortedData = sort(data, sortBy.col, sortBy);

  $: sorter = (column) => {
    if (sortBy.col == column) {
      sortBy.ascending = !sortBy.ascending
    } else {
      sortBy.col = column
      sortBy.ascending = true
    }

    sortedData = sort(sortedData, column, sortBy);
  }
</script>

<section>
  <table>
    <thead>
      <tr>
        <th class='sortable' on:click={sorter('Name')}>Name <i class='icon-sort'></i></th>
        <th class='sortable' on:click={sorter('Status')}>Status <i class='icon-sort'></i></th>
        <th>Owner</th>
        <th>Notes</th>
        <th class='sortable' on:click={sorter('Date created')}>Date created <i class='icon-sort'></i></th>
        <th class='sortable' on:click={sorter('Date retired')}>Date retired <i class='icon-sort'></i></th>
        <th>Run location</th>
        <th>Schedule</th>
        <th>Duration</th>
      </tr>
    </thead>
    <tbody>
      {#each sortedData as row}
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
          <td>{row['Date created']}</td>
          <td>{row['Date retired']}</td>
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