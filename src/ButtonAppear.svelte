<script>
  let displayed = false,
    duration = 1;

  $: hidden = !displayed;
</script>

<style>
  button {
    font-family: monospace;
    position: absolute;
  }
  .container {
    position: relative;
    min-height: 30px;
  }
  .facetious {
    position: absolute;
    visibility: hidden;
    z-index: 1;
  }
  .facetious:hover,
  button:hover + .facetious {
    visibility: visible;
  }
</style>

<div class="flex-item" class:displayed>
  <h1>
    <input id="buttonAppear" type="checkbox" bind:checked={displayed} />
    <label for="buttonAppear">Button appearance (dark pattern)</label>
  </h1>
  <div class:hidden>
    <p>Hover the green button.</p>
    <div class="container">
      <button class="green">Click me!!!!</button>
      <button class="red facetious">Mouahahahah!</button>
    </div>
    <p>
      We toggle the
      <code>visibility</code>
      of the button, no element has been moved. Therefore, CLS is not affected.
      <br />
      The CLS is also not suitable for preventing the occurrence of modal
      overlapping content as no element is moved.
    </p>
  </div>
</div>
