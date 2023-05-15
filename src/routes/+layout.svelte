<header>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/solar-system">Solar System</a></li>
      <li><a href="/glass-effect">Glass Effect</a></li>
    </ul>
  </nav>
</header>
<div class="container">
  <slot />
</div>

<style>
  nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 2em;
    background-color: #333;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  nav ul {
    display: flex;
    list-style: none;
    padding: 0;
    margin: 0;
  }
  nav ul li {
    margin: 0 0.5em;
  }
  nav ul li a {
    color: inherit;
    text-decoration: none;
  }
  nav ul li a:hover {
    text-decoration: underline;
  }

  .container {
    margin: 0;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: black;
    overflow: hidden;
  }
</style>
