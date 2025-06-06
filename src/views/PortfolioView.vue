<template>
  <div class="portfolio">
    <div class="portfolio-title">Ваши дипломы:</div>
    <div class="diplomas" ref="diplomas"></div>
    <div class="actions">
      <div class="btn" @click="triggerUpload">Загрузить</div>
      <div class="btn" @click="deleteLast">Удалить</div>
      <input type="file" ref="fileInput" style="display:none;" @change="uploadFiles" accept="image/*,application/pdf" />
    </div>
    <div class="portfolio-img">
      <img src="../assets/image3.jpg" alt="Портфолио" />
    </div>
  </div>
</template>

<script>
import ModalLogin from '../components/ModalLogin.vue';

export default {
  components: {
    ModalLogin
  },
  data() {
    return {
      diplomaLimit: 8
    };
  },
  methods: {
    triggerUpload() {
      this.$refs.fileInput.click();
    },
    uploadFiles(event) {
      const files = event.target.files;
      const diplomas = this.$refs.diplomas;
      if (!files.length || !diplomas) return;

      for (let i = 0; i < files.length; i++) {
        if (diplomas.children.length >= this.diplomaLimit) break;
        const file = files[i];
        const div = document.createElement('div');
        div.className = 'diploma';

        if (file.type.startsWith('image/')) {
          const img = document.createElement('img');
          img.style.maxWidth = '90px';
          img.style.maxHeight = '110px';
          img.src = URL.createObjectURL(file);
          div.appendChild(img);
        } else if (file.type === 'application/pdf') {
          div.textContent = 'PDF';
        } else {
          div.textContent = 'Файл';
        }

        diplomas.appendChild(div);
      }

      event.target.value = '';
    },
    deleteLast() {
      const diplomas = this.$refs.diplomas;
      if (diplomas.children.length > 0) {
        diplomas.removeChild(diplomas.lastElementChild);
      }
    }
  }
};
</script>
