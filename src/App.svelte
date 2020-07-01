<script>
  import LateralTransition from "./LateralTransition.svelte";
  import MarginLateralTransition from "./MarginLateralTransition.svelte";
  import FlexGrow from "./FlexGrow.svelte";
  import InsertingEl from "./InsertingEl.svelte";
  import SelfIFrame from "./SelfIFrame.svelte";
  import ButtonAppear from "./ButtonAppear.svelte";
  import OpacityTransition from "./OpacityTransition.svelte";
  import unique from "unique-selector";

  let cls = 0,
    elementsCls = new Map();

  try {
    const po = new PerformanceObserver(entryList =>
      entryList.getEntries().forEach(e => {
        if (e.hadRecentInput) {
          console.log(e);
        } else {
          console.log("+" + e.value.toFixed(6), e);
          let node = "undefined";
          if (e.sources && e.sources[0] && e.sources[0].node) {
            node = e.sources[0].node;
          }
          if (
            node == "undefined" ||
            (node != "undefined" &&
              node.parentElement.id != "mainEl" &&
              node.parentElement.id != "cls" &&
              node.parentElement.id != "hmi")
          ) {
            const pastValue = elementsCls.has(node)
              ? elementsCls.get(node).value
              : 0;
            elementsCls.set(node, {
              name: unique(node),
              value: pastValue + e.value
            });
            elementsCls = window.elementsCls = new Map(
              [...elementsCls].sort((a, b) => {
                if (a[1].value > b[1].value) {
                  return -1;
                } else if (a[1].value < b[1].value) {
                  return 1;
                } else {
                  return 0;
                }
              })
            );
          }
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

  document.addEventListener("visibilitychange", () => {
    if (document.visibilityState === "hidden" && cls > 0) {
      console.log(`Hidden page => send CLS: ${cls}`, document);
    }
  });

  function resetCls() {
    elementsCls = new Map();
  }

  function getMainContributingEl(elementsCls) {
    const arrEls = [...elementsCls];
    if (arrEls.length == 0) return undefined;
    if (arrEls[0].length < 2) return undefined;
    const name = arrEls[0][1].name;
    return name == "undefined" ? undefined : name;
  }

  $: displayCls = [...elementsCls]
    .reduce((a, c) => a + c[1].value, 0)
    .toFixed(6);
  $: mainContributingElement = getMainContributingEl(elementsCls);
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
<p class="larger" id="hmi">
  <button on:click={resetCls}>reset</button>
  CLS:
  <strong id="cls">{displayCls}</strong>
  <small id="mainEl">
    (
    {#if mainContributingElement}
      {mainContributingElement}
    {:else}use Chrome > 84 to get info on the main contributing node{/if}
    )
  </small>
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
