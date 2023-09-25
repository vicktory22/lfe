<script>
  import ErrorHero from "./ErrorHero.svelte";
  import LoadingHero from "./LoadingHero.svelte";
  import Scoreboard from "./Scoreboard.svelte";

  const fetchAllPicks = fetch(`${import.meta.env.PUBLIC_API_URL}/picks/all`, {
    credentials: "include",
  }).then((r) => {
    if (r.status >= 400) {
      throw new Error(r.statusText);
    }

    return r.json();
  });
</script>

{#await fetchAllPicks}
  <LoadingHero />
{:then picks}
  <Scoreboard {picks} />
{:catch error}
  <ErrorHero {error} />
{/await}
