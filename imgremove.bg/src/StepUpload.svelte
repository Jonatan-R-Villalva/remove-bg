<script lang="ts">
    import { Cloudinary } from '@cloudinary/url-gen';
    import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect';
    import { ImageStatus } from './types.d'; 
    import { imageStatus, modifiedImage, originalImage } from './store';
    import Dropzone from 'dropzone';
    import 'dropzone/dist/dropzone.css';
    import { onMount } from 'svelte';

    const cloudinary = new Cloudinary({
        cloud:{
            cloudName: 'ddeun9v2x'
        },
        url:{
            secure: true
        }
    })

    onMount(()=>{
        const dropzone = new Dropzone("#dropzone",{
            uploadMultiple: false,
            acceptedFiles: ".jpg , .png , .webp , .jpeg",
            maxFiles: 1
        })
        dropzone.on('sending',(file,xhr,formData)=>{
            imageStatus.set(ImageStatus.UPLOADING)
            formData.append('upload_preset','dd14odww');
            formData.append('timestamp',Date.now()/1000);
            formData.append('api_key',161377236574727);
        })
        dropzone.on('success',(file,response)=>{
            const {
                public_id : publicId ,
                secure_url: url
            } = response

            const imageWithoutBackground = cloudinary
            .image(publicId)
            .effect(backgroundRemoval())

            imageStatus.set(ImageStatus.DONE)
            modifiedImage.set(imageWithoutBackground.toURL())
            originalImage.set(url)

            //crear imagen con fondo transparente
            // y guardar en el backgroundImage
        })
        dropzone.on('error',(file,response)=>{
            console.log("ha ido mal");
            console.log(response);
        })
    })
</script>

<form id="dropzone" 
class="shadow-2xl border-dashed bg-orange-100 border-2 border-gray-400 redounded-lg aspect-video w-full flex justify-center items-center flex-col"
action="https://api.cloudinary.com/v1_1/ddeun9v2x/image/upload"
>
    {#if $imageStatus === ImageStatus.READY}
    <button class="pointer-events-none bg-blue-600 rounded-xl 
    text-bold text-white text-xl px-6 py-4">
        Upload files
    </button>
    <strong class="text-lg mt-4 text-gray-500">Or drop a file</strong>

    {:else if $imageStatus === ImageStatus.UPLOADING}
        <strong class="text-lg mt-4 text-gray-500">Uploading files</strong>
    {/if}
</form>