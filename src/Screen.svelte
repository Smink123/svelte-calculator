<script>
  export let answer;
  export let visuals;
  export let dark;

  $: if (dark) {
    window.document.body.classList.add("dark-mode");
  } else {
    window.document.body.classList.remove("dark-mode");
  }
</script>

<div id="screen">
  <div id="screen-divider">
    <div class="lightDarkContainer">
      <div class="ldButton">
        <label class="switch">
          <input type="checkbox" id="modeToggle" bind:checked={dark}/>
          <span class="slider"></span>
        </label>
      </div>
    </div>
    <div id="screen-text">
      <p id="workings">{visuals}</p>
      <p id="answer">{answer}</p>
    </div>
  </div>
</div>

<style src="./screen.css">
  * {
    box-sizing: border-box;
  }
  #screen {
    max-width: 440px;
    height: 300px;
    flex: 1;
    color: #262626;
    padding: 15px;
    text-align: right;
    word-break: break-all;
    font-weight: 300;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: space-between;
  }
  #screen-text {
    height: 230px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  #screen-divider {
    display: flex;
    flex-direction: column;
    width: 100%;
  }
  #workings {
    margin: 0;
    font-size: 1.5rem;
  }
  #answer {
    font-size: 2rem;
  }
  .lightDarkContainer {
    width: 100%;
    height: 34px;
    margin-bottom: 10px;
    display: flex;
    justify-content: flex-start;
    transition:
      background-color 0.2s ease,
      color 0.2s ease,
      font-size 0.2s ease;
  }
  .switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }
  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #dedbda;
    transition: 0.4s;
    border-radius: 34px;
    box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
  }
  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    transition: 0.4s;
    border-radius: 50%;
  }
  input:checked + .slider:before {
    background-color: #474747;
    transform: translateX(26px);
  }
  input:checked + .slider {
    background-color: black;
  }
  input:checked + .slider:before {
    transform: translateX(26px);
  }
  :global(body.dark-mode) #screen-text {
    color: white;
  }
</style>
