<template>
  <div class="editor">
    <div class="imageContainer" id="imageToSave">
      <img
        :src="imgToEdit.url"
        :alt="`Image of {imgToEdit.name}.`"
        class="ImageToEdit"
      />

      <vue-draggable-resizable
        class-name="upperText"
        w="auto"
        h="auto"
        v-bind:style="{ fontSize: upperFontSize + 'px' }"
      >
        {{ upperMessage }}
      </vue-draggable-resizable>

      <vue-draggable-resizable
        class-name="lowerText"
        w="auto"
        h="auto"
        v-bind:style="{ fontSize: lowerFontSize + 'px' }"
      >
        {{ lowerMessage }}
      </vue-draggable-resizable>
    </div>

    <div class="editPanel">
      <div class="inputFields">
        <label for="textTop">Text top:</label>
        <input v-model="upperMessage" placeholder="Type text here" />
      </div>

      <div>
        <label for="upperFontSize">font size</label>
        <input
          type="range"
          v-model="upperFontSize"
          class="custom-range"
          id="upperFontSize"
        />
      </div>

      <div class="inputFields">
        <label for="textBottom">Text bottom:</label>
        <input
          v-model="lowerMessage"
          type="text"
          id="textBottom"
          name="textBottom"
          placeholder="Type text here"
        />
      </div>

      <div>
        <label for="lowerFontSize">font size</label>
        <input
          type="range"
          v-model="lowerFontSize"
          class="custom-range"
          id="lowerFontSize"
        />
      </div>

      <div class="inputFields">
        <label for="save">File name:</label>
        <input
          type="text"
          v-model="saveName"
          id="save"
          name="save"
          :placeholder="imgToEdit.name"
        />
      </div>
      <button
        type="button"
        class="btn btn-success"
        @click="saveImage(saveName)"
      >
        Download
      </button>
    </div>
  </div>
</template>

<script>
import { saveAsJpeg } from "save-html-as-image";

export default {
  name: "Editor",
  props: { imgToEdit: { type: Object } },
  data() {
    return {
      upperMessage: "",
      lowerMessage: "",
      upperFontSize: "",
      lowerFontSize: "",
      saveName: "",
    };
  },
  created() {
    this.saveName = this.imgToEdit.name;
  },
  methods: {
    saveImage: function(saveName) {
      const node = document.getElementById("imageToSave");
      saveAsJpeg(node, { filename: saveName, printDate: false });
    },

    onDrag: function(x, y) {
      this.x = x;
      this.y = y;
    },
  },
};
</script>

<style scoped>
.editor {
  display: flex;
  width: 100%;
  background-color: rgb(190, 204, 204);
}

.imageContainer {
  position: relative;
  margin-right: 25px;
  left: 0;
  top: 0;
  width: 50%;
}

.ImageToEdit {
  /* position: absolute; */
  width: 100%;
}

.upperText,
.lowerText {
  color: black;
  position: absolute;
  font-weight: bold;
  margin-left: 50px;
}

.upperText {
  margin-top: -150px;
  font-size: 40px;
}

.lowerText {
  font-size: 40px;
  margin-top: -50px;
}

.editPanel {
  flex-grow: 1;
}

.inputFields {
  margin-bottom: 10px;
}

.inputFields input {
  width: 100%;
  flex-grow: 1;
}

.btn-success {
  margin: 20px 0;
  width: 100%;
  flex-grow: 1;
}

/* .test {
  font-size: 40px;
  margin: -350px 0 0 0;
  color: black;
  font-weight: bold;
  position: absolute;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
} */

.modal-dialog {
  max-width: 100%;
  margin: 0;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100vh;
  display: flex;
}
</style>
