<script>
  import { onMount } from "svelte";
  import { Styles } from "sveltestrap";
  import { Button, Col, Container, Icon, Row } from "sveltestrap";
  import Fullscreen from "./Fullscreen.svelte";
  import Settings from "./Settings.svelte";

  let open = false;
  let screenWidth;
  let screenHeight;
  let timing = 5; //en nb de seconde
  let session = 20; //en nb de minutes
  let circle = {
    cx: 0,
    cy: 0,
    r: 200,
  };
  let increase = false;
  let askFull = false;
  let labelButton

  const circleResize = async () => {
    let timeout = timing * 100; // par 10 milliemes de seconde
    let delta = screenHeight / 2 / timeout;
    circle.r = 0;

    let debut = Date.now();
    let fin = 0;
    let rmini = 0;
    let onOrder;

    fullScreen();
    onOrder = askFull;

    do {
      increase = !increase;

      let debutCycle = Date.now();
      let finCycle = 0;
      do {
        const promise = new Promise((resolve, reject) => {
          if (onOrder) {
            setTimeout(() => {
              if (increase) {
                circle.r += delta;
              } else {
                rmini = circle.r - delta;
                circle.r = Math.max(rmini, 0);
              }
              resolve(Date.now());
            }, 10);
          } else {
            reject(0);
          }
        });
        finCycle = await promise;
      } while (finCycle - debutCycle < timing * 1000 && finCycle != 0);
      // console.log("cycle", finCycle - debutCycle);
      fin = Date.now();
    } while (fin - debut < session * 60 * 1000 && finCycle != 0);
    // console.log("durée", fin - debut);
    circle.r = 0;

    fullScreen();
  };

  const locate = () => {
    circle.cx = screenWidth / 2;
    circle.r = 0;
    circle.cy = screenHeight / 2;
  };

  onMount(async () => {
    locate();
  });

  const openSettings = () => {
    open = true;
  };

  const fullScreen = () => {
    askFull = !askFull;
  };

  $: if (screenWidth > 0) locate();
  $: askFull?labelButton = "Arrêter":labelButton = "Commencer";
</script>

<svelte:window
  bind:innerWidth={screenWidth}
  bind:innerHeight={screenHeight}
  on:keydown={locate}
/>
<Styles />
<Fullscreen bind:askFull>
  <Settings bind:open bind:timing bind:session />
  <div class="d-flex justify-content-between">
    <div />
    <div>
      <Button color="dark" on:click={circleResize} size="lg">{labelButton}</Button>
    </div>
    <div>
      <Button color="dark" on:click={openSettings} size="lg"
        ><Icon name="gear" /></Button
      >
    </div>
  </div>

  <svg>
    <circle cx={circle.cx} cy={circle.cy} r={circle.r} fill={"orange"} />
  </svg>
</Fullscreen>

<style>
  svg {
    width: 100%;
    height: 90%;
  }

  circle {
    stroke: black;
  }
</style>
