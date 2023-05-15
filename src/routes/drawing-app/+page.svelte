<script lang="ts">
  import { onMount } from 'svelte';

  let canvas: HTMLCanvasElement;
  let width = 380;
  let height = 380;
  let ctx: CanvasRenderingContext2D;
  let brushSize = 10;
  let isPressed = false;
  let color = 'black';
  let x, y;

  onMount(async () => {
    ctx = canvas.getContext('2d');
    ctx.fillStyle = 'white';
    ctx.fillRect(0, 0, width, height);
    ctx.strokeStyle = color;
    ctx.lineWidth = brushSize * 2;
    ctx.lineCap = 'round';
    ctx.fillStyle = color;
  });

  function drawCircle(x, y, pressure = 1) {
    ctx.beginPath();
    ctx.arc(x, y, brushSize * pressure, 0, Math.PI * 2);
    ctx.fillStyle = color;
    ctx.fill();
  }

  function drawLine(x1, y1, x2, y2, pressure = 1) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.strokeStyle = color;
    ctx.lineWidth = brushSize * 2 * pressure;
    ctx.stroke();
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, width, height);
  }

  // Svelte Event modifiers
  // https://svelte.dev/tutorial/event-modifiers
  /*
    The full list of modifiers:

    preventDefault — calls event.preventDefault() before running the handler. Useful for client-side form handling, for example.
    stopPropagation — calls event.stopPropagation(), preventing the event reaching the next element
    passive — improves scrolling performance on touch/wheel events (Svelte will add it automatically where it's safe to do so)
    nonpassive — explicitly set passive: false
    capture — fires the handler during the capture phase instead of the bubbling phase (MDN docs)
    once — remove the handler after the first time it runs
    self — only trigger handler if event.target is the element itself
    trusted — only trigger handler if event.isTrusted is true. I.e. if the event is triggered by a user action.

    You can chain modifiers together, e.g. on:click|once|capture={...}.
  */

  // See also: https://svelte.dev/repl/f4b5f661bb7b40b7bd1272c1f58d2efc?version=3.24.1
  //           Automatically setup on: props with attribute props
</script>

<svelte:head>
  <meta http-equiv="description" content="https://www.youtube.com/shorts/GEJnfwRah-w" />
</svelte:head>

<div class="container">
  <canvas
    bind:this={canvas}
    {width}
    {height}
    on:pointerdown|capture|preventDefault|stopPropagation|self={(e) => {
      isPressed = true;
      x = e.offsetX;
      y = e.offsetY;
    }}
    on:pointermove|preventDefault|stopPropagation|self={(e) => {
      const { pointerType, pressure, tangentialPressure, tiltX, tiltY } = e;
      console.log({ pointerType, pressure, tangentialPressure, tiltX, tiltY });
      if (isPressed && pressure > 0) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;
        drawCircle(x2, y2, pressure);
        drawLine(x, y, x2, y2, pressure);
        x = x2;
        y = y2;
      }
    }}
    on:pointerup|preventDefault|stopPropagation|self={(e) => {
      e.preventDefault();
      e.stopPropagation();
      isPressed = false;
      x = undefined;
      y = undefined;
    }}
    on:touchstart|preventDefault|stopPropagation|self={(e) => {
      e.preventDefault();
      e.stopPropagation();
      // isPressed = true;
      // x = e.touches[0].clientX;
      // y = e.touches[0].clientY;
    }}
    on:wheel={(e) => {
      e.preventDefault();
      e.stopPropagation();
      if (e.deltaY < 0) {
        brushSize = Math.min(50, brushSize + 5);
      } else {
        brushSize = Math.max(5, brushSize - 5);
      }
    }}
  />
  <div class="toolbox">
    <button id="decrease" on:click={() => (brushSize = Math.max(5, brushSize - 5))}>-</button>
    <span id="size">{brushSize}</span>
    <button id="increase" on:click={() => (brushSize = Math.min(50, brushSize + 5))}>+</button>
    <input type="color" bind:value={color} />
    <button id="clear" on:click={clearCanvas}>X</button>
  </div>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

  .container * {
    box-sizing: border-box;
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    background-color: #f5f5f5;
    font-family: 'Roboto', sans-serif;
    margin: 0;
  }

  .container canvas {
    border: 2px solid #b246b4;
    cursor: crosshair;
  }

  .toolbox {
    background-color: #b246b4;
    border: 1px solid #b246b4;
    display: flex;
    width: 385px;
    padding: 1rem;
  }

  .toolbox * {
    background-color: #fff;
    border: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    height: 50px;
    width: 50px;
    margin: 0.25rem;
    padding: 0.25rem;
    cursor: pointer;
  }

  .toolbox > *:last-child {
    margin-left: auto;
  }

  button,
  #size,
  input {
    border-radius: 5px;
  }
</style>
