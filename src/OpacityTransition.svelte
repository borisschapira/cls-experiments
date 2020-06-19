<script>
  import { onMount } from "svelte";

  let 
    opacity = true,
    displayed = false,
    duration = 2;

  $: hidden = !displayed;

  onMount(() => {
    const interval = setInterval(() => {
      opacity = !opacity;
    }, duration * 1000 + 300);

    return () => {
      clearInterval(interval);
    };
  });
</script>

<style>
  .square {
    width: 4rem;
    height: 4rem;
    display: inline-block;
    opacity: 0;
  }
  .opacity {
    opacity: 1;
  }
  .someText {
    margin-left: 2rem;
  }
</style>

<h1>
  <input id="opacTrans" type="checkbox" bind:checked={displayed} />
  <label for="opacTrans">opacity</label>
</h1>
<div class:hidden>
  <p>
    <label for="drange">Transition duration:</label>
    <input id="drange" type="range" bind:value={duration} min="0" max="2" step="0.0001" />
    {duration}s
  </p>
  <span
    class="square red"
    class:opacity
    style=" transition: opacity {duration}s ease;" />
  <span class="someText">Some text.</span>
  <p>
    <code>opacity</code> animation does not produce layout shifts.
  </p>
</div>
