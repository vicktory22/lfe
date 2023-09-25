<script lang="ts">
  export let picks: any[];
</script>

<div class="overflow-x-auto">
  <table class="table">
    <thead>
      <tr>
        <th>Player</th>
        <th>W</th>
        <th>L</th>
        <th>T</th>
        <th>Total</th>
      </tr>
    </thead>
    <tbody>
      {#each picks as pick}
        <tr>
          <td>{pick.name}</td>
          <td>{pick.wins}</td>
          <td>{pick.losses}</td>
          <td>{pick.ties}</td>
          <td>{pick.wins + (pick.ties / 2)}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>
