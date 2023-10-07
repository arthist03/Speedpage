<script>
    let url = '';
    let loading = false;
  
    async function testSpeed() {
      loading = true;
  
      try {
        const response = await fetch(`/api/pagespeed?url=${encodeURIComponent(url)}`);
        const data = await response.json();
        // Handle the data, update the result component
      } catch (error) {
        console.error(error);
      }
  
      loading = false;
    }
  </script>
  
  <div>
    <label for="url">Enter URL:</label>
    <input type="text" id="url" bind:value={url} placeholder="https://example.com">
    <button on:click={testSpeed} disabled={loading}>Test Speed</button>
  </div>
  