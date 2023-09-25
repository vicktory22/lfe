<script lang="ts">
  import { format } from "date-fns-tz";
  import GameTime from "./GameTime.svelte";
  import Spread from "./Spread.svelte";
  import GameDetails from "./GameDetails.svelte";
  import { isBefore } from "date-fns";

  export let game: any;

  const { games, homeTeam, awayTeam, metadata } = game;
</script>

<div class="flex flex-col p-2 mb-2">
  {#if metadata.showDateHeader}
    <div class="mt-8 mb-4 text-md font-bold uppercase">
      {format(metadata.gameTime, "EEEE, MMMM do")}
    </div>
  {/if}

  <div
    class:cursor-pointer={!metadata.disabled}
    class="card bg-base-200 shadow-xl border static"
    on:click={() => window.location.href=`/games/${games.gameId}`}
    on:keydown={() => undefined}
    role="button"
    tabindex="0"
  >
    <div class="card-body px-2 pb-4 pt-2">
      <div class="flex flex-col">
        <div class="flex flex-nowrap justify-between p-2 pb-4">
          <GameTime gameTime={metadata.gameTime} />

          {#if isBefore(new Date(), metadata.gameTime)}
            <Spread
              spread={games.spread}
              homeTeam={homeTeam.abbreviation}
              awayTeam={awayTeam.abbreviation}
            />
          {/if}
        </div>
        <div>
          <GameDetails {awayTeam} {homeTeam} />
        </div>
      </div>
    </div>
  </div>
</div>
