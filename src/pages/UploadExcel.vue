<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="main">
          <div
            class="dropzone-container"
            @dragover="dragover"
            @dragleave="dragleave"
            @drop="drop"
          >
            <input
              type="file"
              name="file"
              id="fileInput"
              class="hidden-input"
              @change="onChange"
              ref="file"
            />

            <label for="fileInput" class="file-label">
              <div v-if="isDragging">Release to drop files here.</div>
              <div v-else>Drop files here or <u>click here</u> to upload.</div>
            </label>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isDragging: false,
      files: [],
    };
  },
  methods: {
    onChange() {
      this.files.push(...this.$refs.file.files);
      this.convertBase64(this.$refs.file.files[0]);
    },
    dragover(e) {
      e.preventDefault();
      this.isDragging = true;
    },
    dragleave() {
      this.isDragging = false;
    },
    drop(e) {
      e.preventDefault();
      this.$refs.file.files = e.dataTransfer.files;
      this.onChange();
      this.isDragging = false;
    },
    convertBase64(file) {
      let reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = function () {
        console.log(reader.result);
      };
      reader.onerror = function (error) {
        console.log('Error: ', error);
      };
    }
  }
}
</script>
<style lang="scss">
.upgrade-table-btn {
  width: 160px;
  &:first-child {
    margin-right: 10px;
  }
}
.main {
  display: flex;
}

.dropzone-container {
  padding: 4rem;
  background: #f7fafc;
  border: 1px solid #e2e8f0;
}

.hidden-input {
  opacity: 0;
  overflow: hidden;
  position: absolute;
  width: 1px;
  height: 1px;
}

.file-label {
  font-size: 20px;
  display: block;
  cursor: pointer;
}

.preview-container {
  display: flex;
  margin-top: 2rem;
}

.preview-card {
  display: flex;
  border: 1px solid #a2a2a2;
  padding: 5px;
  margin-left: 5px;
}

.preview-img {
  width: 50px;
  height: 50px;
  border-radius: 5px;
  border: 1px solid #a2a2a2;
  background-color: #a2a2a2;
}
</style>
