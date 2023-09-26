<script lang="ts">
  import ErrorHero from "./ErrorHero.svelte";
  import LoadingHero from "./LoadingHero.svelte";
  import SelectGame from "./SelectGame.svelte";

  export let gameId: string;
  
  const fetchGame = fetch('https://lbe-production.vicktory22.workers.dev/games', {
    credentials: "include",
  })
    .then((r) => {
      if (r.status === 401) {
        window.location.href = "/login";
      }

      if (r.status >= 400) {
        throw new Error(r.statusText);
      }

      return r.json();
    })
    .then((games) => {
      return games
        .filter((game: any) => { 
          return game.games.gameId == gameId 
        });
    });
</script>

<div class="static">
{#await fetchGame}
  <LoadingHero />
{:then game}
  <SelectGame game={game.at(0)} />
{:catch error}
  <div>
  <ErrorHero {error} />
  </div>
{/await}
</div>
