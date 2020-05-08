<template>
  <div class="editor">
    <div class="imageContainer" id="imageToSave">
      <img
        :src="imgToEdit.url"
        :alt="`Image of {imgToEdit.name}.`"
        class="ImageToEdit"
      />

      <drag-it-dude>
        <div
          class="upperText"
          v-bind:style="{ fontSize: upperFontSize + 'px' }"
        >
          {{ upperMessage }}
        </div>
      </drag-it-dude>

      <drag-it-dude class="lowerMsgContainer">
        <div
          class="lowerText"
          v-bind:style="{ fontSize: lowerFontSize + 'px' }"
        >
          {{ lowerMessage }}
        </div>
      </drag-it-dude>
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
import DragItDude from "vue-drag-it-dude";

export default {
  name: "Editor",
  components: {
    DragItDude,
  },
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
  },
};
</script>

<style scoped>
.editor {
  display: flex;
  align-items: flex-start;
  align-content: flex-start;
  width: 100%;
  background-color: rgb(190, 204, 204);
}

.imageContainer {
  display: inline-block;
  position: relative;
  margin-right: 25px;
  left: 0;
  top: 0;
  width: 60%;
}

.ImageToEdit {
  width: 100%;
}

.upperText,
.lowerText {
  color: black;
  font-weight: bold;
}

.upperText {
  font-size: 40px;
}

.lowerText {
  font-size: 40px;
}

.lowerMsgContainer {
  padding-top: 30px;
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

@media only screen and (max-width: 1000px) {
  .editor {
    display: inline-block;
  }

  .imageContainer {
    width: 100%;
  }

  .editPanel {
    margin: 0 10px;
    margin-left: 10px;
  }
}
</style>
