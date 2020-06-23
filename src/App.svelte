<script>
  import LateralTransition from "./LateralTransition.svelte";
  import MarginLateralTransition from "./MarginLateralTransition.svelte";
  import FlexGrow from "./FlexGrow.svelte";
  import InsertingEl from "./InsertingEl.svelte";
  import SelfIFrame from "./SelfIFrame.svelte";
  import ButtonAppear from "./ButtonAppear.svelte";
  import OpacityTransition from "./OpacityTransition.svelte";

  let cls = 0;

  $: displayCls = cls.toFixed(6);

  try {
    const po = new PerformanceObserver(entryList =>
      entryList.getEntries().forEach(e => {
        if (!e.hadRecentInput) {
          console.log(e.value, e);
          cls += e.value;
        }
      })
    );

    po.observe({
      type: "layout-shift",
      buffered: true
    });
  } catch (e) {
    console.log(e);
  }

  function resetCls() {
    cls = 0;
  }
</script>

<style>
  :global(label) {
    display: inline-block;
  }

  :global(.hidden) {
    display: none;
  }

  :global(.red) {
    background-color: red;
  }

  :global(.green) {
    background-color: green;
  }

  :global(.blue) {
    background-color: #f00;
  }

  :global(.displayed) {
    order: 1;
  }

  :global(.flex-container) {
    display: flex;
    flex-direction: column-reverse;
  }

  :global(.flex-item) {
    display: block;
    flex: 1;
  }

  :global(.flex-item h1) {
    margin: 0;
  }

  :global(.nba) {
    min-height: 200vh;
  }

  .larger {
    font-size: xx-large;
  }

  small {
    font-size: 0.7em;
  }
</style>

<h1>
  Cumulative Layout Shift Experiments
  <br />
  <small>
    based on the
    <a href="https://wicg.github.io/layout-instability/">
      Layout Instability API
    </a>
    &nbsp;·&nbsp;
    <a href="https://github.com/borisschapira/cls-experiments">source code</a>
    &nbsp;·&nbsp; made by
    <a href="https://twitter.com/boostmarks">boostmarks</a>
    with
    <span aria-label="love">❤️</span>
  </small>
</h1>
<p class="larger">
  <button on:click={resetCls}>reset</button>
  CLS:
  <strong id="cls">{displayCls}</strong>
  <br />
</p>

<div class="flex-container">
  <InsertingEl />
  <ButtonAppear />
  <SelfIFrame />
  <FlexGrow />
  <OpacityTransition />
  <LateralTransition />
  <MarginLateralTransition />
</div>
