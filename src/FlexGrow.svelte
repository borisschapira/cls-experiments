<script>
  let displayed = false,
    duration = 1;

  $: hidden = !displayed;
</script>

<style>
  .container {
    width: 100%;
    display: flex;
  }
  .container p {
    flex: 1;
    color: white;
    padding: 2rem;
  }

  .container p.growing:hover {
    flex: 2;
  }
</style>

<div id="flexGrowXp" class="flex-item" class:displayed>
  <h1>
    <input id="flexGrow" type="checkbox" bind:checked={displayed} />
    <label for="flexGrow">
      Growing (
      <code>ease</code>
      vs.
      <code>linear</code>
      )
    </label>
  </h1>
  <div class:hidden>
    <p>Hover green elements.</p>
    <div class="container">
      <p class="red">Element 1</p>
      <p class="green growing" style="transition: flex {duration}s ease;">
        Ease
      </p>
      <p class="red">Element 3</p>
    </div>
    <div class="container">
      <p class="red">Element 1</p>
      <p class="green growing" style="transition: flex {duration}s linear;">
        Linear
      </p>
      <p class="red">Element 3</p>
    </div>
    <p>
      Flex elements are in flow so layout shif should appear.
      <br />
      But
      <strong>
        if the viewport is thin enough, they don't. Because of
        <a href="https://wicg.github.io/layout-instability/#sec-unstable-nodes">
          the 3-pixel-unit rule
        </a>
      </strong>
      <br />
      With
      <code>ease</code>
      , CLS increments is much bigger during the growing, not the shrinking.
    </p>
  </div>
</div>
