<template>
  <div v-if="currentImagePath == ''"  class="image_viewer">
    <img class="no_image" width="360" src="@/assets/NO_IMAGE.png"/>
  </div>
  <div v-else class="image_viewer">
    <!-- <img :src=fullPath /> -->
    <img width="360" :src="base64" />
    <p>{{ currentImagePath }}</p>
  </div>
</template>

<script lang="ts">
import fs from "fs";
import { defineComponent } from "vue";
import { inject } from "vue";

export default defineComponent({
  name: "ImageViewer",
  setup() {
    // 依存性の注入 https://v3.vuejs.org/guide/composition-api-provide-inject.html#adding-reactivity
    const directoryPath = inject("directoryPath", "");
    const currentImagePath = inject("currentImagePath", "");
    return {
      directoryPath,
      currentImagePath,
    };
  },
  props: {
    buttonLabel: String,
  },
  data() {
    return {};
  },
  methods: {},
  watch: {},
  computed: {
    displayImage (): boolean {
      if (this.currentImagePath == ""){
        return true
      }
      else {
        return false
      }
    },
    fullPath(): string {
      //return "file://" + this.directoryPath + "/" + this.currentImagePath;
      return this.directoryPath + "/" + this.currentImagePath;
    },
    base64(): string {
      if (this.fullPath == "") {
        return "";
      }
      try {
        const binary = fs.readFileSync(this.fullPath);
        return (
          "data:image/png;base64," + Buffer.from(binary).toString("base64")
        );
      } catch (err) {
        console.log(err);
        return "";
      }
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
img {
  max-width: 80%;
  height: auto;
}
</style>
