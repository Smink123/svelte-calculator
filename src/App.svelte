<script>
  import Screen from "./Screen.svelte";

  let answer = "";
  let equation = [];
  let num = "";
  let visuals = "";
  let continuation = false;
  let dark = false;

  function operator(event) {
    if (continuation) {
      visuals = answer;
      visuals += event.target.textContent;
      equation = [answer, event.target.textContent];
    } else {
      visuals += event.target.textContent;
      equation = [...equation, Number(num), event.target.textContent];
    }
    answer = "";
    num = "";
    continuation = false;
  }

  function value(event) {
    num += event.target.textContent;
    visuals += event.target.textContent;
    answer = "";
  }

  function clear() {
    answer = "";
    equation = [];
    num = "";
    visuals = "";
    continuation = false;
  }

  function undo() {
    visuals = visuals.substring(0, visuals.length - 1);
    num = num.substring(0, num.length - 1);
  }

  function calc() {
    equation = [...equation, Number(num)];
    num = "";
    let aa = equation.join("");
    answer = Function(`'use strict'; return (${aa})`)();
    equation = [answer];
    continuation = true;
  }

  $: if (dark) {
    window.document.body.classList.add("dark-mode");
  } else {
    window.document.body.classList.remove("dark-mode");
  }
</script>

<section>
  <div id="calc-shape">
    <Screen {answer} {visuals} {dark} />
    <div id="calc-buttons">
      <div class="four-rows">
        <button on:click={operator} class="operators">+</button>
        <button on:click={operator} class="operators">-</button>
        <button on:click={operator} class="operators">*</button>
        <button on:click={operator} class="operators">/</button>
      </div>
      <div class="four-rows">
        <button on:click={value} class="numbers">7</button>
        <button on:click={value} class="numbers">8</button>
        <button on:click={value} class="numbers">9</button>
        <button on:click={undo} class="operators">←</button>
      </div>
      <div id="bottom-calc">
        <div id="left">
          <div class="three-rows">
            <button on:click={value} class="numbers">4</button>
            <button on:click={value} class="numbers">5</button>
            <button on:click={value} class="numbers">6</button>
          </div>
          <div class="three-rows">
            <button on:click={value} class="numbers">1</button>
            <button on:click={value} class="numbers">2</button>
            <button on:click={value} class="numbers">3</button>
          </div>
          <div class="three-rows">
            <button on:click={value} class="operators" id="dot">.</button>
            <button on:click={value} class="numbers">0</button>
            <button on:click={clear} class="operators">DEL</button>
          </div>
        </div>
        <div id="right">
          <button on:click={calc} id="equal">=</button>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  :root {
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    line-height: 1.5;
    font-weight: 400;

    color-scheme: light dark;
    color: rgba(255, 255, 255, 0.87);
    background-color: #242424;

    font-synthesis: none;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  section {
    width: 100vw;
    height: 100vh;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  button {
    border-radius: 8px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    cursor: pointer;
    transition: border-color 0.25s;
  }
  button:hover {
    border-color: #646cff;
  }
  button:focus,
  button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
  }

  @media (prefers-color-scheme: light) {
    :root {
      color: #213547;
      background-color: #ffffff;
    }
    button {
      background-color: #f9f9f9;
    }
  }
  * {
    box-sizing: border-box;
  }
  .numbers,
  .operators {
    height: 80px;
    width: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 5px;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    font-weight: 200;
    font-size: 1.5rem;
  }
  .numbers {
    background-color: white;
  }
  .operators {
    background-color: #dddbda;
  }
  .four-rows {
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 25%;
    justify-content: space-around;
    align-items: center;
  }
  .three-rows {
    display: flex;
    flex-direction: row;
    width: 100%;
    justify-content: space-around;
    align-items: center;
  }
  #bottom-calc {
    display: flex;
    flex-direction: row;
  }
  #left {
    width: 75%;
    display: flex;
    flex-direction: column;
  }
  #right {
    width: 25%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  #equal {
    background-color: #4a73f0ff;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 97%;
    width: 80px;
    border-radius: 60px;
    margin: 5px;
    cursor: pointer;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  }
  #calc-buttons {
    max-width: 440px;
  }
  #calc-shape {
    padding: 20px;
    width: 425px;
    background-color: #ededed;
    border-radius: 15px;
    transition: background-color 0.2s ease;
  }
  :global(body.dark-mode) {
    background-color: #0a0a0a;
    transition: background-color 0.2s ease;
  }
  :global(body.dark-mode) .numbers {
    background-color: #141414;
    color: #f5f5f5;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    transition:
      background-color 0.2s ease,
      color 0.2s ease;
  }
  :global(body.dark-mode) .operators {
    background-color: #474747;
    color: #f5f5f5;
    transition:
      background-color 0.2s ease,
      color 0.2s ease;
  }
  :global(body.dark-mode) #calc-shape {
    background-color: #2e2e2e;
    transition: background-color 0.2s ease;
  }
</style>
