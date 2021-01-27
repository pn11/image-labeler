<template>
  <div class="conrtol_buttons" >
    <button v-on:click="nextImage">{{ nextLabel }}</button
    > <button v-on:click="previousImage">{{ previousLabel }}</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { inject } from "vue";

export default defineComponent({
  name: "ControlButtons",
  setup() {
    // 依存性の注入 https://v3.vuejs.org/guide/composition-api-provide-inject.html#adding-reactivity
    const directoryPath = inject("directoryPath", "");
    const currentImagePath = inject("currentImagePath", "");
    const currentImageIdx = inject("currentImageIdx", 0);
    const imageList = inject("imageList", [""]);
    return {
      directoryPath,
      currentImagePath,
      currentImageIdx,
      imageList
    };
  },
  created() {
    // EventHandling https://dev.to/focusedlabs/add-keyboard-shortcuts-to-your-vue-app-22jg?signin=true
    window.addEventListener("keydown", this.keyListener);
  },
  unmounted() {
    window.removeEventListener("keydown", this.keyListener);
  },
  props: {
    nextLabel: String,
    previousLabel: String,
  },
  data() {
    return {
    };
  },
  methods: {
    nextImage() {
      if (this.currentImageIdx < this.imageList.length - 1){
        this.currentImageIdx += 1
      }
      else {
        this.currentImageIdx = 0
      }
    },
    previousImage() {
      if (this.currentImageIdx > 0 ){
        this.currentImageIdx -= 1
      }
      else {
        this.currentImageIdx = this.imageList.length - 1
      }
    },
    keyListener(event: KeyboardEvent) {
      // console.log(event.key)
      if (event.key === "ArrowLeft") {
        this.nextImage()
      }
      if (event.key === "ArrowRight") {
        this.previousImage()
      }    
      console.log(this.currentImageIdx)  
    }
  },
  watch: {
    currentImageIdx(){
      this.currentImagePath = this.imageList[this.currentImageIdx]
    }
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
</style>
