<script lang="ts">
  export let players: string[];
</script>

<div class="overflow-x-auto">
  <table class="table">
    <thead>
      <tr>
        <th>Player</th>
      </tr>
    </thead>
    <tbody>
      {#each players as player}
        <tr>
          <td>{player}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>
