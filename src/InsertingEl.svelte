<script>
  let displayed = false,
    duration = 600;

  $: hidden = !displayed;

  function prependLi() {
    insertLi(false);
  }
  function appendLi() {
    insertLi(true);
  }
  function insertLi(append = false) {
    [...document.getElementsByClassName("new")].forEach(e =>
      e.classList.remove("new")
    );

    const newLi = document.createElement("li");
    newLi.classList.add("new");
    newLi.appendChild(document.createTextNode("Element"));

    const myUl = document.getElementById("list");

    setTimeout(() => {
      if (append) {
        myUl.appendChild(newLi);
      } else {
        myUl.insertBefore(newLi, myUl.firstChild);
      }
      setTimeout(() => {
        newLi.classList.remove("new");
      }, 200);
    }, duration);
  }

  function resetUl() {
    const myUl = document.getElementById("list");
    while (myUl.firstChild) {
      myUl.removeChild(myUl.firstChild);
    }
  }
</script>

<style>
  :global(.new) {
    color: rgb(89, 159, 0);
  }
  .hidden {
    display: none;
  }
  :global(li) {
    transition: color 0.5s ease;
  }
</style>

<h1>
  <input id="insertEl" type="checkbox" bind:checked={displayed} />
  <label for="insertEl">Inserting elements in a list</label>
</h1>
<div class:hidden>
  <p>
    Prepending an element in a list moves the others and creates layout shifts.
    Appending an element does not.
    <br />
    <small>
      <em>
        <strong>Note:</strong>
        There's a default 600&nbsp;ms delay between the click and the insertion
        to make sure the layout shifts are not considered as related to the
        click.
        <br />
        <strong>Interestingly:</strong>
        If your FID is short, the layout-shift happens near the user action.
        <strong>If your FID is &gt;500ms… your CLS improves.</strong>
      </em>
    </small>
  </p>
  <p>
    Delay duration:
    <input type="range" bind:value={duration} min="0" max="1000" step="10" />
    {duration}&nbsp;ms
  </p>
  <button on:click={prependLi}>
    Prepend
    <code>li</code>
    element
  </button>
  <button on:click={appendLi}>
    Append
    <code>li</code>
    element
  </button>
  <button on:click={resetUl}>
    Empty
    <code>ul</code>
  </button>
  <ol id="list">
    <li>Element</li>
    <li>Element</li>
    <li>Element</li>
  </ol>
</div>
