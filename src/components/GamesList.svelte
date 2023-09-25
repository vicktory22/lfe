<script lang="ts">
  import { format } from "date-fns-tz";
  import { fromUnixTime, isAfter } from "date-fns";
  import Game from "./Game.svelte";

  export let games: any[] = [];

  let latestGameDate = new Date();

  const parsedGames = games?.map((game) => {
    const gameTime = fromUnixTime(game.games.gameTime);
    const disabled = isAfter(new Date(), gameTime);
    let showDateHeader = false;

    if (format(latestGameDate, "yyyyMMdd") !== format(gameTime, "yyyyMMdd")) {
      latestGameDate = gameTime;
      showDateHeader = true;
    }

    return {
      ...game,
      metadata: {
        gameTime,
        disabled,
        showDateHeader,
      },
    };
  });
</script>

{#each parsedGames as game}
  <Game {game} />
{/each}
