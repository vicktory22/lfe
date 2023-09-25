<script lang="ts">
  import TeamWithSpread from "./TeamWithSpread.svelte";
  import ErrorHero from "./ErrorHero.svelte";

  export let game: any;

  let error: Error;

  async function makePick(homeOrAway: string) {
    await fetch(`${import.meta.env.PUBLIC_API_URL}/picks`, {
      method: "POST",
      credentials: "include",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        team_id:
          homeOrAway === "home" ? game.homeTeam.teamId : game.awayTeam.teamId,
        spread: homeOrAway === "home" ? game.games.spread : -game.games.spread,
        game_id: game.games.gameId,
      }),
    })
      .then(handleNetworkErrors)
      .then(handleSuccess)
      .catch(handleError);
  }

  function handleSuccess() {
    window.location.href = "/";
  }

  async function handleNetworkErrors(response: Response) {
    if (response.status === 401) {
      window.location.href = "/login";
    }

    if (response.status >= 400) {
      const errResponse = await response.json();
      throw new Error(errResponse.message);
    }

    return response;
  }

  function handleError(pageError: Error) {
    error = pageError;
    closeModal();
  }

  function closeModal() {
    document.getElementById("cancelBtnHome")?.click();
    document.getElementById("cancelBtnAway")?.click();
  }

  function showModal(teamId: number) {
    document.getElementById(teamId.toString())?.showModal();
  }

  function cancel() {
    window.location.href = "/games";
  }
</script>

{#if error}
  <ErrorHero {error} />
{:else}
  <div class="flex flex-col justify-center h-96 w-full border-opacity-50">
    <div
      on:click={() => showModal(game.awayTeam.teamId)}
      on:keypress={() => undefined}
      role="button"
      tabindex="0"
    >
      <TeamWithSpread team={game.awayTeam} spread={-game.games.spread} />
    </div>

    <div class="divider">OR</div>

    <div
      on:click={() => showModal(game.homeTeam.teamId)}
      on:keypress={() => undefined}
      role="button"
      tabindex="0"
    >
      <TeamWithSpread team={game.homeTeam} spread={game.games.spread} />
    </div>
  </div>

  <div class="px-2">
    <button
      class="btn btn-error btn-outline btn-block"
      on:click={() => cancel()}>Cancel</button
    >
  </div>
{/if}

<dialog id={game.homeTeam.teamId} class="modal">
  <div class="modal-box">
    <h3 class="text-lg text-center font-bold">CONFIRM PICK</h3>
    <div class="divider" />
    <div class="flex flex-col justify-center items-center">
      <div class="w-16"><img src={game.homeTeam.logo} alt="team logo" /></div>
      <h3 class="text-lg text-center">
        Spread: <span class="font-bold">{game.games.spread}</span>
      </h3>
    </div>
    <div class="divider" />
    <div class="modal-action">
      <form method="dialog">
        <button
          class="btn btn-primary mr-4"
          on:click|preventDefault={() => makePick("home")}
        >
          Confirm</button
        >
        <button id="cancelBtnHome" class="btn btn-error btn-outline">Cancel</button>
      </form>
    </div>
  </div>
</dialog>

<dialog id={game.awayTeam.teamId} class="modal">
  <div class="modal-box">
    <h3 class="text-lg text-center font-bold">CONFIRM PICK</h3>
    <div class="divider" />
    <div class="flex flex-col justify-center items-center">
      <div class="w-16"><img src={game.awayTeam.logo} alt="team logo" /></div>
      <h3 class="text-lg text-center">
        Spread: <span class="font-bold">{-game.games.spread}</span>
      </h3>
    </div>
    <div class="divider" />
    <div class="modal-action">
      <form method="dialog">
        <button
          class="btn btn-primary mr-4"
          on:click|preventDefault={() => makePick("away")}
        >
          Confirm</button
        >
        <button id="cancelBtnAway" class="btn btn-error btn-outline">Cancel</button>
      </form>
    </div>
  </div>
</dialog>
