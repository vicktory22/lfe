<script lang="ts">
  export let error: Error | undefined;
</script>

<div class="hero min-h-full z-0">
  <div class="hero-content flex flex-col items-center text-center py-8">
    <div class="max-w-md py-8 text-error font-bold">
      <p>Oh no!</p>
      <p>{error?.message}</p>
    </div>
    <button
      class="btn btn-error btn-outline btn-sm"
      on:click={() => (window.location.href = "/")}>Go Home</button
    >
  </div>
</div>
