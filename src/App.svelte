<script>
  import LateralTransition from "./LateralTransition.svelte";
  import MarginLateralTransition from "./MarginLateralTransition.svelte";
  import FlexGrow from "./FlexGrow.svelte";
  import InsertingEl from "./InsertingEl.svelte";
  import SelfIFrame from "./SelfIFrame.svelte";

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
  </small>
</h1>
<p>
  <button on:click={resetCls}>reset</button>
  CLS:
  <strong id="cls">{displayCls}</strong>
  <br />
</p>

<MarginLateralTransition />
<LateralTransition />
<FlexGrow />
<SelfIFrame />
<InsertingEl />
