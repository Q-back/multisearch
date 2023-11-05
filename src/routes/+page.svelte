<script lang="ts">
  let inputQuery = '';
  let searchQuery = '';

  // TypeScript debounce function
  function debounce(func: (value: string) => void, wait: number) {
    let timeoutId: ReturnType<typeof setTimeout>;
    return function executedFunction(value: string) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => func(value), wait);
    };
  }

  // Update searchQuery with a debounced call
  const debouncedUpdateQuery = debounce((value: string) => {
    searchQuery = value;
  }, 300);

  // When the user types, update the inputQuery and call the debounced update
  function handleInput(event: Event) {
    const target = event.target as HTMLInputElement;
    inputQuery = target.value;
    debouncedUpdateQuery(inputQuery);
  }
</script>

<style>
  .iframe-container {
    display: flex;
    justify-content: space-between;
  }

  iframe {
    flex: 1; /* Each iframe will take up an equal amount of space */
    height: 300px;
    border: none;
    overflow: auto; /* Enables scrolling */
    margin: 0 5px; /* Adds a little space between the iframes */
  }
</style>

<div>
  <input
    type="text"
    placeholder="Enter search query"
    value={inputQuery}
    on:input={handleInput}
  />
  <button on:click={() => debouncedUpdateQuery(inputQuery)}>Search</button>
</div>

{#if searchQuery}
  <div class="iframe-container">
    <iframe
      title="Google Search"
      src={`https://www.google.com/webhp?igu=1&q=${encodeURIComponent(searchQuery)}`}
    ></iframe>
    <iframe
      title="Bing Search"
      src={`https://www.bing.com/search?q=${encodeURIComponent(searchQuery)}`}
    ></iframe>
  </div>
{/if}
