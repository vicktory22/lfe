<script lang="ts">
  import ErrorHero from "./ErrorHero.svelte";
  import GamesList from "./GamesList.svelte";
  import LoadingHero from "./LoadingHero.svelte";

  const fetchGames = fetch('https://lbe-production.vicktory22.workers.dev/games', {
    credentials: "include",
  }).then((r) => {
    if (r.status === 401) {
      window.location.href = "/login";
    }

    if (r.status >= 400) {
      throw new Error(r.statusText);
    }

    return r.json();
  });
</script>

{#await fetchGames}
  <LoadingHero />
{:then games}
  <GamesList {games} />
{:catch error}
  <ErrorHero {error} />
{/await}
