<svelte:head>
  <title>Glass Effect</title>
  <meta name="description" content="https://www.youtube.com/shorts/P2sKUQTWLZs" />
</svelte:head>
<div class="box">
  <div class="glass" />
</div>

<style>
  .box {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    background: url('/wooden-floor-texture-vector-175355.jpg');
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }

  .glass {
    width: 250px;
    height: 250px;
    backdrop-filter: blur(10px);
    border-radius: 30px;
    background: linear-gradient(100deg, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2));
    box-shadow: 0 10px 26px 0 rgba(0, 0, 0, 0.5);
  }
</style>
