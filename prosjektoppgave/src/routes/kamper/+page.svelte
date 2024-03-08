<script>
    let selectedRowIndex = null;
  
    function handleRowClick(index) {

        selectedRowIndex = index;

    }
  </script>
  
  <style>
    .selected {
      background-color: lightblue;
    }
  </style>
  
  <table>
    <tr
      on:click={() => handleRowClick(0)}
      class:selected={selectedRowIndex === 0}
    >
      <td>Rad 1</td>
    </tr>
    <tr
      on:click={() => handleRowClick(1)}
      class:selected={selectedRowIndex === 1}
    >
      <td>Rad 2</td>
    </tr>
    <tr
      on:click={() => handleRowClick(2)}
      class:selected={selectedRowIndex === 2}
    >
      <td>Rad 3</td>
    </tr>
  </table>
  