<script>
  import { onMount } from "svelte";

  let searchValue = "";
  let books = [];
  let isLoading = false;
  let errorMessage = "";

  async function searchBooks() {
    isLoading = true;
    errorMessage = "";
    try {
      if (searchValue.trim() === "") {
        books = [];
      } else {
        const response = await fetch(
          `http://openlibrary.org/search.json?title=${encodeURIComponent(
            searchValue
          )}`
        );
        if (!response.ok) {
          throw new Error("Error en la búsqueda");
        }
        const data = await response.json();
        books = data.docs.slice(0, 10);
      }
    } catch (error) {
      errorMessage = error.message;
      books = [];
    } finally {
      isLoading = false;
    }
  }
</script>

<div class="search-container">
  <input type="text" bind:value={searchValue} placeholder="Buscar..." />
  <button on:click={searchBooks}>Buscar</button>
</div>

{#if isLoading}
  <p>Cargando...</p>
{/if}

{#if errorMessage}
  <p>{errorMessage}</p>
{/if}

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
