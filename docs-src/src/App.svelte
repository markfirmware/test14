<div>
  {#if repos}
    test repos
    <ul>
      {#each repos as repo}
        {#if repo.name.includes("test")}
          <li><a href={repo.html_url}>{repo.name}</a></li>
        {/if}
      {/each}
    </ul>
  {/if}
</div>

<script>
  import {onMount} from 'svelte';
  let repos;
  let repos_buffer = [];
  let next = (page_number) => {
    fetch("https://api.github.com/users/markfirmware/repos?page=" + page_number)
      .then(r => r.json())
      .then(j => {
        if (j.length == 0) {
          repos = repos_buffer;
        } else {
          repos_buffer = repos_buffer.concat(j);
          next(page_number + 1);
        }
      });
  };
  next(1);
  onMount(() => {
  });
</script>

<style>
  :global(body) {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
  }
  .App {
    text-align: center;
  }
  .App code {
    background: #0002;
    padding: 4px 8px;
    border-radius: 4px;
  }
  .App p {
    margin: 0.4rem;
  }

  .App-header {
    background-color: #f9f6f6;
    color: #333;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: calc(10px + 2vmin);
  }
  .App-link {
    color: #ff3e00;
  }
  .App-logo {
    height: 36vmin;
    pointer-events: none;
    margin-bottom: 3rem;
    animation: App-logo-spin infinite 1.6s ease-in-out alternate;
  }
  @keyframes App-logo-spin {
    from {
      transform: scale(1);
    }
    to {
      transform: scale(1.06);
    }
  }
</style>

