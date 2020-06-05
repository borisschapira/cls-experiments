<script>
  import { onMount } from "svelte";

  let bouncing = false,
    displayed = false,
    duration = 2,
    vhidden = false,
    white = false;

  $: hidden = !displayed;

  onMount(() => {
    const interval = setInterval(() => {
      bouncing = !bouncing;
    }, 2300);

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
  }

  .red {
    background-color: red;
  }

  .hidden {
    display: none;
  }

  .vhidden {
    visibility: hidden;
  }

  .white {
    background-color: white !important;
  }

  .bouncing {
    margin-left: 40vw;
  }
  .someText {
    margin-left: 2rem;
  }
</style>

<h1>
  <input id="mtranslate" type="checkbox" bind:checked={displayed} />
  <label for="mtranslate">marginX (in flow)</label>
</h1>
<div class:hidden>
  <p>
    Transition duration:
    <input type="range" bind:value={duration} min="0" max="2" step="0.0001" />
    {duration}s
    <br />
    <input id="vhidden" type="checkbox" bind:checked={vhidden} />
    <label for="vhidden">
      add
      <code>visibility: hidden</code>
    </label>
    <br />
    <input id="ws" type="checkbox" bind:checked={white} />
    <label for="ws">make the square white</label>
  </p>
  <span
    class="square red"
    class:bouncing
    class:vhidden
    class:white
    style=" transition: margin-left {duration}s ease;" />

  <p>
    Modifying the margin creates layout-shifts… even if
    <strong>no element is "pushed" but the animated one.</strong>
    <br />
    If you make the square
    <code>white</code>
    , it still produces layout-shifts, even if nothing's visible to the user.
    <br />
    If you make the square
    <code>visibility: hidden</code>
    , however, no layout-shift.
  </p>
</div>
