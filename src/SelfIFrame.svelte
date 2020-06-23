<script>
  let displayed = false;

  $: hidden = !displayed;
</script>

<div class="flex-item" class:displayed>
  <h1>
    <input id="iFrame" type="checkbox" bind:checked={displayed} />
    <label for="iFrame">iFrame</label>
  </h1>
  <div class:hidden>
    <p>
      Activate experiments in the
      <code>iframe</code>
      .
    </p>
    <iframe class="nba" title="self" src="./" width="100%" />
    <p>
      The layout shifts that happen in an iframe are not counted in the main
      page CLS. Contexts are separated.
    </p>
  </div>
</div>
