<script lang="ts">
  export let singleScrollPoint = 0;

  let thisEl;
  let relativeTop;

  function scrollYEffect(singleScrollPoint) {
    console.log(singleScrollPoint);
  }

  $: relativeTop = (singleScrollPoint + 200) / window.innerHeight;
</script>

<div class="content-container" bind:this={thisEl}>
  <div
    class="hello"
    style:background="none"
    style:color="black"
    style:right="{-Math.sin(relativeTop * Math.PI) * 150}px"
  >
    1.
  </div>
  <div class="hello" style:background="hsl(231, 69%, 15%)">2.</div>
  <div class="hello" style:background="hsl(350, 84%, 64%)">3.</div>
  <div class="hello" style:background="hsl(257, 29%, 26%)">4.</div>
  <div class="hello" style:background="hsl(149, 29%, 38%)">5.</div>
</div>

<style>
  .hello {
    font-size: 100px;
    text-align: center;
    color: white;
    height: 500px;
    display: flex;
    justify-content: left;
    align-items: center;
    position: relative;
  }
</style>
