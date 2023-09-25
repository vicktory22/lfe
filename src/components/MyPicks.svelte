<script lang="ts">
  export let picks: any[];

  function calculateWinLoss(pick: any) {
    const { picks, games } = pick;

    const { spread, teamId } = picks;

    const { awayTeamId, awayTeamScore, homeTeamId, homeTeamScore } = games;

    if (teamId === awayTeamId) {
      if ((awayTeamScore + spread) > homeTeamScore) {
        return "W";
      }

      if ((awayTeamScore + spread) === homeTeamScore) {
        return "T";
      }
    }

    if (teamId === homeTeamId) {
      if ((homeTeamScore + spread) > awayTeamScore) {
        return "W";
      }

      if ((homeTeamScore + spread) === awayTeamScore) {
        return "T";
      }
    }

    return "L";
  }
</script>

<div class="overflow-x-auto">
  <table class="table">
    <thead>
      <tr>
        <th>Week</th>
        <th>Pick</th>
        <th>Spread</th>
        <th>W/L/T</th>
      </tr>
    </thead>
    <tbody>
      {#each picks as pick}
        <tr>
          <td>{pick.picks.weekId}</td>
          <td>{pick.teams.displayName}</td>
          <td>{pick.picks.spread}</td>
          <td>{calculateWinLoss(pick)}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>
