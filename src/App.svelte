<script>
  let searchValue = "";
  let books = [];

  async function searchBooks() {
    if (searchValue.trim() === "") {
      books = [];
      return;
    }

    const response = await fetch(
      `http://openlibrary.org/search.json?title=${encodeURIComponent(
        searchValue
      )}`
    );
    const data = await response.json();
    books = data.docs.slice(0, 10);
  }
</script>

<div class="search-container">
  <input type="text" bind:value={searchValue} placeholder="Buscar..." />
  <button on:click={searchBooks}>Buscar</button>
</div>
<ul>
  {#each books as book (book.key)}
    <li>
      <strong>{book.title}</strong>
      <p>Autor: {book.author_name && book.author_name.join(", ")}</p>
    </li>
  {/each}
</ul>

<style>
  .search-container {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    padding: 10px;
    background-color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  ul {
    list-style-type: none;
    padding: 0;
    margin-top: 60px;
    min-width: "80%";
  }
  li {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
  }
  input {
    padding: 5px;
    margin-right: 5px;
  }
  button {
    padding: 5px;
  }
</style>
