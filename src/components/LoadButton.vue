<template>
  <div class="load_button">
    <button v-on:click="selectDirectory">{{ buttonLabel }}</button>
    <p>Current Directory: {{ directoryPath }}</p>
    <p>Images:</p>
    <ul id="images">
      <li v-for="image in images" :key="image">
        {{ image }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import fs from "fs";
import { defineComponent } from "vue";
import { remote } from "electron";
import isImage from "is-image";

export default defineComponent({
  name: "LoadButton",
  props: {
    buttonLabel: String,
  },
  data() {
    return {
      directoryPath: "",
      images: [""],
      imagesStr: "",
    };
  },
  methods: {
    selectDirectory() {
      // https://www.electronjs.org/docs/api/dialog
      // http://m-miya.blog.jp/archives/1070822761.html
      remote.dialog
        .showOpenDialog(remote.getCurrentWindow(), {
          //filters: [
          //  { name: "Text File", extensions: ["txt"] },
          //  { name: "All Files", extensions: ["*"] },
          //],
          properties: ["openDirectory"],
        })
        .then((result) => {
          if (result.canceled) {
            return;
          }
          this.directoryPath = result.filePaths[0];
        })
        .catch((err) => {
          console.log(err);
        });
    },
    listDirectory(directory: string) {
      console.log(directory);
      fs.readdir(directory, { withFileTypes: true }, (err, dirents) => {
        if (err != null) {
          console.log(err);
          return;
        }
        this.images = dirents
          .filter((dirent) => dirent.isFile())
          .map(({ name }) => name)
          .filter((name) => isImage(name));
        this.imagesStr = this.images.join("\n");
      });
    },
  },
  watch: {
    directoryPath() {
      this.listDirectory(this.directoryPath);
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
</style>
