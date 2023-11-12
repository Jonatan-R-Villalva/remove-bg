<script lang="ts">
  import "two-up-element";
  import { originalImage, modifiedImage } from "./store";

  let processingImage = true;
  let tries = 0;
  let intervarId: any;

  $: {
    if (processingImage) {
      clearInterval(intervarId);
      intervarId = setInterval(() => {
        tries++;
      }, 500);
    }
  }
</script>

<div class="inline-flex w-full">
  <div class="w-3/6">
    <two-up>
      <img src={$originalImage} alt="Imagen original subida por el usuario" />
      <img
        on:load={() => (processingImage = false)}
        on:error={() => (processingImage = true)}
        src={`${$modifiedImage}&t=${tries}`}
        alt="imagen sin fondo subida por el usuario"
      />
    </two-up>
  </div>
  <div class="w-3/6 flex justify-center">
    <a
      download
      href={$modifiedImage}
      class="bg-blue-500 h-10 font-bold mt-10 text-xl text-center hover:bg-blue-700 py-2 px-4 text-white rounded-lg"
    >
      Download image without background
    </a>
  </div>
</div>
