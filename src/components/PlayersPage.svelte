<script lang="ts">
  import ErrorHero from "./ErrorHero.svelte";
  import LoadingHero from "./LoadingHero.svelte";
  import Players from "./Players.svelte";

  const fetchPlayers = fetch('https://lbe-production.vicktory22.workers.dev/players', {
    credentials: "include",
  }).then<string[]>((r) => {
    if (r.status === 401) {
      window.location.href = "/login";
    }

    if (r.status >= 400) {
      throw new Error(r.statusText);
    }

    return r.json();
  });
</script>

{#await fetchPlayers}
  <LoadingHero />
{:then players}
  <Players {players} />
{:catch error}
  <ErrorHero {error} />
{/await}
