<script>
  import { onMount } from "svelte";

  let bouncing = false,
    vhidden = false,
    opacity = 1,
    white = false,
    displayed = false,
    duration = 2;

  $: hidden = !displayed;

  onMount(() => {
    const interval = setInterval(() => {
      bouncing = !bouncing;
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
  }

  .vhidden {
    visibility: hidden;
  }

  .white {
    background-color: white !important;
  }

  .bouncing {
    transform: translateX(40vw);
  }
  .someText {
    margin-left: 2rem;
  }
</style>

<h1>
  <input id="ltranslate" type="checkbox" bind:checked={displayed} />
  <label for="ltranslate">translateX (out of flow)</label>
</h1>
<div class:hidden>
  <p>
    <label for="drange">Transition duration:</label>
    <input id="drange" type="range" bind:value={duration} min="0" max="2" step="0.0001" />
    {duration}s
    <br />
    <label for="orange">Opacity:</label>
    <input id="orange" type="range" bind:value={opacity} min="0" max="1" step="0.0001" />
    {opacity}
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
    style=" transition: transform {duration}s ease;opacity:{opacity};" />
  <span class="someText">Some text.</span>
  <p>
    Using a translation does not create layout shifts
    <strong>except when the duration is 0.</strong>
    <br />
    <strong>In that case</strong>, if you make the square
    <code>white</code>
    , it still produces layout-shifts, even if nothing's visible to the user.
    <br />
    Same thing if you change the <code>opacity</code> of the element.
    <br />
    If you make the square
    <code>visibility: hidden</code>
    , however, no layout-shift.
  </p>
</div>
