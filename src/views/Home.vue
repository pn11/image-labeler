<template>
  <div class="home">
    <LoadButton buttonLabel="Select Directory"/>
    <ImageViewer/>
    <ControlButtons nextLabel="<" previousLabel=">"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { provide, reactive, ref } from 'vue'
import LoadButton from "@/components/LoadButton.vue"; // @ is an alias to /src
import ImageViewer from "@/components/ImageViewer.vue";
import ControlButtons from "@/components/ControlButtons.vue";

export default defineComponent({
  name: "Home",
  data() {
    return {
    }
  },
  components: {
    LoadButton,
    ImageViewer,
    ControlButtons
  },
  setup() {
    // 依存性の注入 https://v3.vuejs.org/guide/composition-api-provide-inject.html#adding-reactivity
    const directoryPath = ref("")
    const currentImagePath = ref("")
    provide("directoryPath", directoryPath)
    provide("currentImagePath", currentImagePath)
  }
});
</script>
