<template>
  <div class="home">
    <LoadButton buttonLabel="Select Directory"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import LoadButton from "@/components/LoadButton.vue"; // @ is an alias to /src

export default defineComponent({
  name: "Home",
  components: {
    LoadButton
  }
});
</script>
