<template>
  <div class="conrtol_buttons" >
    <button v-on:click="previousImage">{{ previousLabel }}</button> <button v-on:click="nextImage">{{ nextLabel }}</button>
    <ul class="label_buttons">
      <li v-for="(dir, idx) in dirList" :key="dir">
        {{ idx+1 }}: <button v-on:click="moveImage(dir)">{{ dir }}</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import fs from "fs";
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
    const dirList = inject("dirList", [""]);
    return {
      directoryPath,
      currentImagePath,
      currentImageIdx,
      imageList,
      dirList
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
      console.log(this.currentImageIdx + " / " + this.imageList.length) 
    },
    previousImage() {
      if (this.currentImageIdx > 0 ){
        this.currentImageIdx -= 1
      }
      else {
        this.currentImageIdx = this.imageList.length - 1
      }
      console.log(this.currentImageIdx + " / " + this.imageList.length) 
    },
    moveImage(dir: string) {
      const srcPath = this.directoryPath + "/" + this.currentImagePath;
      const tgtPath = this.directoryPath + "/" + dir + "/" + this.currentImagePath;
      fs.rename(srcPath, tgtPath, (err) => {
        if (err != null) {
          console.log(err);
          return
        } 
        console.log(srcPath + " -> " + tgtPath);
      });
      this.imageList = this.imageList.filter(n => n !== this.currentImagePath);
      this.currentImageIdx = Math.min(this.currentImageIdx, this.imageList.length - 1)
      this.currentImagePath = this.imageList[this.currentImageIdx]
    },
    keyListener(event: KeyboardEvent) {
      // console.log(event.key)
      if (event.key == "ArrowLeft") {
        this.nextImage()
      }
      if (event.key == "ArrowRight") {
        this.previousImage()
      }
      const maxShortcutId = Math.min(this.dirList.length, 9);
      for (let i = 1; i <= maxShortcutId; i++){
        if (event.key == String(i)){
          this.moveImage(this.dirList[i-1]);
        }
      }
    }
  },
  watch: {
    currentImageIdx(){
      if (this.imageList.length == 0) {
        this.currentImagePath = "";
        return
      }
      this.currentImagePath = this.imageList[this.currentImageIdx]
      console.log(this.currentImageIdx + " / " + this.imageList.length) 
    },
    imageList(){
      console.log("list: ", this.imageList.length)
      if (this.imageList.length == 0) {
        this.currentImagePath = "";
      }
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
ul.label_buttons li {
  display: list-item;
}
</style>
