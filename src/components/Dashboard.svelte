<script>
  import ErrorHero from "./ErrorHero.svelte";
  import LoadingHero from "./LoadingHero.svelte";
  import MyPicks from "./MyPicks.svelte";

  const fetchPicks = fetch('https://lbe-production.vicktory22.workers.dev/picks', {
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

{#await fetchPicks}
  <LoadingHero />
{:then picks}
  <h1 class="p-2 font-bold">My Picks</h1>
  <MyPicks {picks} />
{:catch error}
  <ErrorHero {error} />
{/await}
