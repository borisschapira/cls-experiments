<script>
  let displayed = false;
  
  $: hidden = !displayed;

</script>

<h1>
  <input id="iFrame" type="checkbox" bind:checked={displayed} />
  <label for="iFrame">iFrame</label>
</h1>
<iframe class:hidden title="self" src="./"></iframe>

