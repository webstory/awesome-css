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

  function drawCircle(x, y) {
    ctx.beginPath();
    ctx.arc(x, y, brushSize, 0, Math.PI * 2);
    ctx.fillStyle = color;
    ctx.fill();
  }

  function drawLine(x1, y1, x2, y2) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.strokeStyle = color;
    ctx.lineWidth = brushSize * 2;
    ctx.stroke();
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, width, height);
  }
</script>

<svelte:head>
  <meta http-equiv="description" content="https://www.youtube.com/shorts/GEJnfwRah-w" />
</svelte:head>

<div class="container">
  <canvas
    bind:this={canvas}
    {width}
    {height}
    on:mousedown={(e) => {
      isPressed = true;
      x = e.offsetX;
      y = e.offsetY;
    }}
    on:mouseup={(e) => {
      isPressed = false;
      x = undefined;
      y = undefined;
    }}
    on:mousemove={(e) => {
      if (isPressed) {
        const x2 = e.offsetX;
        const y2 = e.offsetY;
        drawCircle(x2, y2);
        drawLine(x, y, x2, y2);
        x = x2;
        y = y2;
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