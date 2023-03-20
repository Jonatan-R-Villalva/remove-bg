<script lang="ts">
    import "two-up-element";
    import { originalImage , modifiedImage } from "./store";

    let processingImage = true;
    let tries = 0;
    let intervarId : any;

    $: {
        if(processingImage){
            clearInterval(intervarId);
            intervarId = setInterval(()=>{
                tries++
            },500)
        }
    }
</script>

<two-up>
    <img 
    src={$originalImage} alt="Imagen original subida por el usuario">
    <img
    on:load={ ()=> (processingImage = false) }
    on:error={ ()=> (processingImage = true) }
    src={`${$modifiedImage}&t=${tries}`} alt="imagen sin fondo subida por el usuario">
</two-up>

<a download href={$modifiedImage} class="bg-blue-500 w-full font-bold text-xl text-center hover:bg-blue-700 text-white rounded-full px-4 py-2">
    Descargar imagen sin fondo
</a>