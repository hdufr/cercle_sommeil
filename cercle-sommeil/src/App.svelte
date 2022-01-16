<script>
  import { onMount } from "svelte";

  let screenWidth;
  let screenHeight;
  let timing = 5; //en nb de secondes
  let circle = {
    cx: 0,
    cy: 0,
    r: 200,
  };
  let increase = false;

  const circleResize = async () => {
    let timeout = timing * 100; // par 10 milliemes de seconde
    let delta = screenHeight / 2 / timeout;
    circle.r = 0;

    let debut = Date.now();
    let fin = 0;
    let rmini = 0;

    do {
      increase = !increase;

      let debutCycle = Date.now();
      let finCycle = 0;
      do {
        const promise = new Promise((resolve, reject) => {
          setTimeout(() => {
            if (increase) {
              circle.r += delta;
            } else {
              rmini = circle.r - delta;
              circle.r = Math.max(rmini, 0);
            }
            resolve(Date.now());
          }, 10);
        });
        finCycle = await promise;
      } while (finCycle - debutCycle < timing * 1000);
      console.log("cycle", finCycle - debutCycle);
      fin = Date.now();
    } while ((fin - debut < 60 * 1000));
    console.log("durée", fin - debut);
	circle.r = 0;
  };

  const locate = () => {
    circle.cx = screenWidth / 2;
    circle.r = 0;
    circle.cy = screenHeight / 2;
  };

  onMount(async () => {
    locate();
  });

  $: if (screenWidth > 0) locate();
</script>

<svelte:window
  bind:innerWidth={screenWidth}
  bind:innerHeight={screenHeight}
  on:keydown={locate}
/>
<div>
  <label for="cycle">Temps en seconde d'un cycle de respiration</label>
  <input bind:value={timing} id="cycle" />
  <button on:click={circleResize}> Start </button>
</div>
<svg>
  <circle cx={circle.cx} cy={circle.cy} r={circle.r} fill={"orange"} />
</svg>

<style>
  :global(body) {
    background-color: rgb(17, 16, 16);
  }

  div {
    color: white;
    display: inline-flex;
  }

  label {
    padding: 5px;
    height: 30px;
	margin: 0 10px 0 0;
  }

  input {
    color: white;
    /* padding: 5px; */
    margin: 0 10px 0 0;
    background-color: transparent;
    width: 50px;
  }

  button {
	color: white;
	margin: 0 10px 0 10px;
    background-color: rgb(99, 21, 21);
  }

  svg {
    width: 100%;
    height: 100%;
  }

  circle {
    stroke: black;
  }
</style>
