<script>
	import { spring } from 'svelte/motion';

  let position = "absolute";

  let X = 0;
  let Y = 0;

  let cursor;

  let coords = spring(
    { x: 0, y: 0 },
    {
      stiffness: 0.05,
      damping: 0.25
    }
  );

  let size = spring(1); 

  function updateCursor () {
    coords.set({ x: X, y: Y});

    let cursorHalfWidth = cursor.offsetWidth/2;

    cursor.style.left = $coords.x - cursorHalfWidth + "px";
    cursor.style.top = $coords.y - cursorHalfWidth + "px";

    cursor.style.position = position;

  }

  let isFull = false;
  let scale = 1;
  function handleClick () {
    isFull = !isFull;

    let cursorHalfWidth = cursor.offsetWidth/2;

    if (isFull) {

      scale = 2;
      cursor.style.width = `${scale * 14}rem`;
      cursor.style.height  = `${scale * 14}rem`;

      cursorHalfWidth = cursor.offsetWidth/2;
      cursor.style.left = $coords.x - cursorHalfWidth + "px";
      cursor.style.top = $coords.y - cursorHalfWidth + "px";
      return
    }

    scale = 1;
    cursor.style.width = `${scale * 14}rem`;
    cursor.style.height  = `${scale * 14}rem`;

    cursorHalfWidth = cursor.offsetWidth/2;
    cursor.style.left = $coords.x - cursorHalfWidth + "px";
    cursor.style.top = $coords.y - cursorHalfWidth + "px";
    return

  }

</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div 
  class="
    relative overflow-hidden
    h-screen w-full flex items-center justify-center cursor-none
    bg-[url(/images/Ectoplasma_invisible.jpg)] bg-cover bg-center bg-fixed
  
  "
  on:mouseenter={(e) => {
    cursor.style.display = "block";

  }}
  on:mouseleave={(e) => {
    cursor.style.display = "none";
  }}
>
  <div
    bind:this={cursor}
    class="
      absolute w-56 h-56 z-1
      bg-[url(/images/Ectoplasma2.gif)] bg-cover bg-center bg-fixed
      border-4 border-stone-50 rounded-full
    "
  >

  </div>
</div> 


<svelte:window
	on:mousemove={(e) => {
    X = e.pageX;
    Y = e.pageY;
    updateCursor()
	}}
	on:mousedown={(e) => {
		handleClick()
	}}
	on:mouseup={(e) => {
		handleClick()
	}}

/>