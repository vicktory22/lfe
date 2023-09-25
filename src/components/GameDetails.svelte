<script>
export let awayTeam;
export let homeTeam;
</script>

<div class="flex justify-around">
  <div class="flex-1 flex flex-col justify-around items-center">
    <div class="avatar static">
      <div class="w-8">
        <img src={ awayTeam.logo } alt="away team logo" />
      </div>
    </div>
    <div class="font-bold">{ awayTeam.shortName }</div>
  </div>

  <div class="text-center text-xl font-bold">@</div>

  <div class="flex-1 flex flex-col justify-center items-center">
    <div class="avatar static">
      <div class="w-8">
        <img src={ homeTeam.logo } alt="home team logo" />
      </div>
    </div>
    <div class="font-bold">{ homeTeam.shortName }</div>
  </div>
</div>
