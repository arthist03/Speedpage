<script>
    let results = null;
  </script>
  
  {#if results}
    <div>
      <h2>Performance Results</h2>
      <p>Speed Score: {results.lighthouseResult.categories.performance.score * 100}</p>
      <!-- Display other relevant data from the results -->
    </div>
  {:else}
    {#if results === null}
      <p>Enter a URL and click "Test Speed" to get results.</p>
    {:else}
      <p>Test failed. Please check the URL and try again.</p>
    {/if}
  {/if}
  