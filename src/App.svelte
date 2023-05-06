<script>
  import { onMount } from "svelte";

  let data = [];
  let searchValue = "";
  let filteredData = [];

  async function fetchData() {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    data = await response.json();
    filteredData = data;
  }

  function filterData() {
    filteredData = data.filter((item) => {
      return Object.values(item).some((value) =>
        value.toString().toLowerCase().includes(searchValue.toLowerCase())
      );
    });
  }

  onMount(fetchData);
</script>

<input
  type="text"
  bind:value={searchValue}
  placeholder="Buscar..."
  on:input={filterData}
/>
<button on:click={filterData}>Buscar</button>
<ul>
  {#each filteredData as item (item.id)}
    <li>
      <strong>{item.title}</strong>
      <p>{item.body}</p>
    </li>
  {/each}
</ul>
