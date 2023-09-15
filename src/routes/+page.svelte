<script lang="ts">
  import { onMount } from "svelte";

  let testGrid = [
    [8, 8, 8, 9],
    [8, 8, 8, 8],
    [8, 8, 8, 8],
    [0, 8, 8, 8],
  ];

  let mouse: HTMLDivElement;

  let drawCursor: HTMLDivElement;
  let cursorTop: number, cursorLeft: number;
  let cursorDrag = false;

  onMount(() => {
    console.log(document.getElementById("start").parentElement.offsetTop);
    let start = document.getElementById("start");
    drawCursor.style.left =
      start.offsetLeft + start.parentElement.offsetLeft + "px";
    drawCursor.style.top =
      start.offsetTop + start.parentElement.offsetTop + "px";
    drawCursor.style.cursor = "none";
  });

  function clickCursor(e: MouseEvent) {
    e.preventDefault();
    cursorDrag = true;
  }

  function releaseCursor(e: MouseEvent) {
    e.preventDefault();
    cursorDrag = false;
  }

  function drag(e: MouseEvent) {
    if (cursorDrag) {
      if (Math.abs(e.movementX) > Math.abs(e.movementY)) {
        drawCursor.style.left = drawCursor.offsetLeft + e.movementX + "px";
      } else {
        drawCursor.style.top = drawCursor.offsetTop + e.movementY + "px";
      }
    }
    cursorLeft = e.pageX - drawCursor.clientWidth / 4;
    cursorTop = e.pageY - drawCursor.clientHeight / 4;
  }
</script>

<svelte:head>
  <title>The Witness Practice</title>
</svelte:head>

<div class="flex flex-col h-screen">
  <div class="flex flex-row justify-center m-4 h-full">
    <div class="flex flex-col p-4 gap-4 bg-base-300 rounded-xl mt-4 h-fit">
      {#each testGrid as row}
        <div class="flex flex-row gap-4">
          {#each row as col}
            <div
              class="relative flex flex-col text-center w-24 h-24 bg-base-200"
            >
              {#if col == 0}
                <div
                  id="start"
                  class="absolute zero rounded-full bg-base-300 w-12 h-12"
                />
              {:else if col == 9}
                <div
                  class="absolute nine rounded-full bg-base-300 w-4 h-8 rounded-full"
                />
              {/if}
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
  <div
    bind:this={drawCursor}
    class="absolute w-12 h-12 bg-yellow-400 rounded-full"
    on:mousedown={clickCursor}
  />
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div
    bind:this={mouse}
    class="cursor"
    style="left: {cursorLeft}px; top: {cursorTop}px; user-select: none;"
  />
</div>

<svelte:window
  on:mousemove={drag}
  on:mouseup={releaseCursor}
  on:contextmenu={(e) => {
    e.preventDefault();
  }}
/>

<style>
  .zero {
    left: -2rem;
    bottom: -2rem;
  }

  .nine {
    right: -1rem;
    top: -2rem;
  }
</style>
