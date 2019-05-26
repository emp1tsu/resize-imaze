<template>
  <div class="container">
    <label
      v-show="!uploadedImage"
      class="input-item_label"
    >画像を選択
      <input
        type="file"
        @change="onFileChange"
      >
    </label>
    <div class="preview-item">
      <img
        v-show="uploadedImage"
        :src="uploadedImage"
        class="preview-item-file"
        alt=""
      />
      <div
        v-show="uploadedImage"
        @click="remove"
        class="preview-item-btn"
      >
        <p class="preview-item-name">{{ img_name }} ×</p>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'home',
  components: {
    HelloWorld,
  },
  data() {
    return {
      uploadedImage: '',
      img_name: ''
    }
  },
  methods: {
    onFileChange (e) {
      const files = e.target.files;
      this.createImage(files[0]);

      // 拡張子の前と後ろで分ける
      const imgNameExe = files[0].name.split('.');
      const imgName = imgNameExe[0];
      const imgExe = imgNameExe[1];

      // 最大表示数 全角：１０文字 半角：２０文字
      const maxBytes = 20;
      const imgNameBytes = encodeURIComponent(imgName).replace(/%../g,"x").length;

      this.img_name = files[0].name;
    },
    createImage(file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        this.uploadedImage = e.target.result;
      };

      if (file) reader.readAsDataURL(file);
    },
    remove () {
      this.uploadedImage = '';
    }
  }
};
</script>

<style scoped>
label > input {
  display: none;
}

label {
  padding: 0 1rem;
  border: solid 1px #888;
} 

label::after {
  content: '+';
  font-size: 1rem;
  color: #888;
  padding-left: 1rem;
}
</style>
