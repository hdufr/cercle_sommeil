<!-- Fullscreen.svelte -->
<script>
  import { onMount } from "svelte";

  // define initial component state
  let isFull = true;
  let start = false;
  let fsContainer = null;
  export let askFull = false;

  // boring plain js fullscreen support stuff below
  const noop = () => {};

  const fullscreenSupport = !!(
    document.fullscreenEnabled ||
    document.webkitFullscreenEnabled ||
    document.mozFullScreenEnabled ||
    document.msFullscreenEnabled ||
    false
  );

  const exitFullscreen = (
    document.exitFullscreen ||
    document.mozCancelFullScreen ||
    document.webkitExitFullscreen ||
    document.msExitFullscreen ||
    noop
  ).bind(document);

  const requestFullscreen = () => {
    const requestFS = (
      fsContainer.requestFullscreen ||
      fsContainer.mozRequestFullScreen ||
      fsContainer.webkitRequestFullscreen ||
      fsContainer.msRequestFullscreen ||
      noop
    ).bind(fsContainer);
    requestFS();
  };

  onMount(() => {
    // Add the icon stylesheet dynamically
    const link = document.createElement("link");
    link.rel = "stylesheet";
    link.href = "https://fonts.googleapis.com/icon?family=Material+Icons";
    document.head.appendChild(link);

    // remove the link when component is unmounted
    return () => {
      link.parentNode.removeChild(link);
    };
  });

  // handler for the fullscreen button
  const fsToggle = () => {
    if(!start){start = true; return}
    if (!fullscreenSupport) return;

    if (!askFull) {
      exitFullscreen();
    } else {
      requestFullscreen(fsContainer);
    }
    isFull = !isFull;
  };

  $: askFull ? fsToggle():fsToggle() ;
</script>

<div class="fs" class:isFull bind:this={fsContainer}>
  <slot {isFull} />
</div>

<style>

</style>
