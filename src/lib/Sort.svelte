<script context="module">
  export function sort(data, column, sortBy) {
    // if (sortBy.col == column) {
    //   sortBy.ascending = !sortBy.ascending
    // } else {
    //   sortBy.col = column
    //   sortBy.ascending = true
    // }
    
    // Modifier to sorting function for ascending or descending
    let sortModifier = (sortBy.ascending) ? 1 : -1;
    
    let sort = (a, b) => 
      (a[column] < b[column]) 
      ? -1 * sortModifier 
      : (a[column] > b[column]) 
      ? 1 * sortModifier 
      : 0;
    
      data = data.sort(sort);
      return data;
  }
</script>