<script>
  import Papa from 'papaparse'

  let vizData = [];
  Papa.parse('https://docs.google.com/spreadsheets/d/e/2PACX-1vRqBuIYkkzFIMlq541njH9S9mO-nda40sab-AMP4tlhcWFiZt0QrcXOVS44ALJtDW17yu53vyhbUEuY/pub?gid=0&single=true&output=csv', {
    header: true,
    download: true,
    complete: function(results) {
      vizData = results.data
    }
  })

  let pipelineData = [];
  Papa.parse('https://docs.google.com/spreadsheets/d/e/2PACX-1vRqBuIYkkzFIMlq541njH9S9mO-nda40sab-AMP4tlhcWFiZt0QrcXOVS44ALJtDW17yu53vyhbUEuY/pub?gid=87687849&single=true&output=csv', {
    header: true,
    download: true,
    complete: function(results) {
      pipelineData = results.data
    }
  })

  let sortBy = {col: 'Name', ascending: true};
  
  $: sort = (column, data) => {
    
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
    
    if (data=='pipeline')
      pipelineData = pipelineData.sort(sort);
    else
      vizData = vizData.sort(sort);
  }
</script>

<main>
  <div class='header'>
    <h1>Data Visualization and Pipeline Directory</h1>
    <a href="https://centre.humdata.org" target="_blank" rel="noreferrer"> 
      <img src="images/centreforHumdata_green_TransparentBG.png" class="logo" alt="Centre for Humanitarian Data Logo" />
    </a>
  </div>

  <section>
    <h2>Data Visualizations</h2>
    <table>
      <thead>
        <tr>
          <th on:click={sort('Name')}>Name</th>
          <th on:click={sort('Status')}>Status</th>
          <th>Owner</th>
          <th>Notes</th>
          <th>Code</th>
          <th>Scraper</th>
          <th>Datasheet</th>
        </tr>
      </thead>
      <tbody>
        {#each vizData as row}
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


  <section>
    <h2>Pipelines</h2>
    <table>
      <thead>
        <tr>
          <th on:click={sort('Name', 'pipeline')}>Name</th>
          <th on:click={sort('Status', 'pipeline')}>Status</th>
          <th>Owner</th>
          <th>Notes</th>
          <th>Run location</th>
          <th>Schedule</th>
          <th>Duration</th>
        </tr>
      </thead>
      <tbody>
        {#each pipelineData as row}
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

</main>

<style>
  .logo {
    height: 4em;
  }
  .header {
    display: flex;
  }
  .header a {
    margin-left: auto;
    margin-top: auto;
  }
  table td.Active {
    color: #0063B3;
    font-weight: bold;
  }
  table td.InDevelopment {
    color: #18998F;
    font-weight: bold;
    text-wrap: nowrap;
  }
  table td.Retired {
    color: #F2645A;
    font-weight: bold;
  }
</style>
