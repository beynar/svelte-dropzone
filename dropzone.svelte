<script>
  import { onMount } from "svelte";
  import Dropzone from "dropzone";
  export let dropzoneEvents = {};
  export let options = { previewTemplate: "<div/>" };
  export let dropzoneClass = "dropzone";
  export let hooveringClass = "dropzone-hoovering";
  export let id = "dropId";
  export let autoDiscover = false;

  onMount(() => {
    const dropzoneElement = document.getElementById(id);
    if (!options.previewTemplate) {
      options.previewTemplate = "<div/>";
    }
    if (!options.dictDefaultMessage) {
      options.dictDefaultMessage = "";
    }

    let svDropzone = new Dropzone(`div#${id}`, {
      ...options
    });
    if (autoDiscover !== true) {
      Dropzone.autoDiscover = false;
    }

    svDropzone.on("addedfile", f => {
      dropzoneElement.classList.remove(hooveringClass);
    });
    svDropzone.on("dragenter", e => {
      console.log(dropzoneElement);
      dropzoneElement.classList.toggle(hooveringClass);
    });
    svDropzone.on("dragleave", e => {
      dropzoneElement.classList.toggle(hooveringClass);
    });
    Object.entries(dropzoneEvents).map(([eventKey, eventFunc]) =>
      svDropzone.on(eventKey, eventFunc)
    );

    if (options.clickable !== false) {
      dropzoneElement.style.cursor = "pointer";
    }
    svDropzone.on("error", (file, errorMessage) => {
      console.log(`Error: ${errorMessage}`);
    });
  });
</script>

<style>
  .dropzone {
    height: 300px;
    background: #fdfdfd;
    border-radius: 5px;
    border: 2px dashed #ff3e00;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all 300ms ease-out;
  }

  .dropzone.dropzone-hoovering {
    border: 2px solid #ff3e00;
    background: rgba(255, 62, 0, 0.05);
  }
</style>

<div action="#" class={dropzoneClass} {id}>
  <slot />
  <input hidden name="sites_data" type="file" />
</div>
