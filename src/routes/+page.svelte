<script lang="ts">
  import { onMount } from "svelte";

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;
  let walkFrames: HTMLImageElement[] = [];
  let frameIndex = 0;
  const frameDelay = 150;
  let lastFrameTime = 0;

  // Load frames
  function loadFrames() {
    for (let i = 0; i < 10; i++) {
      const img = new Image();
      const n = i.toString().padStart(3, "0");
      img.src = `/sprites/2_enemies_1_WALK_${n}.png`;
      walkFrames.push(img);
    }
  }

  function loop(timestamp: number) {
    if (timestamp - lastFrameTime > frameDelay) {
      frameIndex = (frameIndex + 1) % walkFrames.length;
      console.log(frameIndex);
      lastFrameTime = timestamp;
    }

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.drawImage(walkFrames[frameIndex], 100, 100);

    requestAnimationFrame(loop);
  }

  onMount(() => {
    const context = canvas.getContext("2d");
    if (!context) throw new Error("Canvas 2D context is null");
    ctx = context;

    loadFrames();
    // Wait for the first image to load before starting
    walkFrames[0].onload = () => requestAnimationFrame(loop);
  });
</script>

<canvas
  bind:this={canvas}
  width="640"
  height="480"
  style="border:1px solid black;"
></canvas>
