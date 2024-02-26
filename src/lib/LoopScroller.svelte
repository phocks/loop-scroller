<script lang="ts">
  import { onMount } from "svelte";
  import debounce from "lodash.debounce";

  export let component;

  const DEBOUNCE_TIME = 100; // in milliseconds (time between scroll events)

  let times = 10;
  let components = Array(times).fill(component);
  let containerScrollY = 0;
  let drift = 0;

  let height = 0;
  let midPoint = 0;

  $: midPoint = height / 2;
  $: drift = midPoint - containerScrollY;

  let container: HTMLDivElement;

  onMount(() => {
    height = container.scrollHeight;
    container.scrollTo(0, height / 2);
  });

  function handleScroll(event: Event) {
    containerScrollY = container.scrollTop;

    if (containerScrollY < 200) {
      resetPosition(midPoint, drift);
    } else if (containerScrollY > height - window.innerHeight - 200) {
      resetPosition(midPoint, drift);
    }
  }

  function resetPosition(midPoint: number, drift: number) {
    container.scrollTo(0, midPoint - (drift % (height / times)));
  }

  const debouncedResetPosition = debounce(resetPosition, DEBOUNCE_TIME);

  $: debouncedResetPosition(midPoint, drift);
</script>

<div bind:this={container} class="container" on:scroll={handleScroll}>
  {#each components as Component}
    <svelte:component this={Component} />
  {/each}
</div>

<style lang="scss">
  .container {
    background-color: #f0f0f0;
    height: 100dvh;
    overflow: scroll;
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .container::-webkit-scrollbar {
    display: none;
  }
</style>
