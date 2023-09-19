<template>
    <div id="imageGallery">
      <h1>imageGallery</h1>
      <div v-if="uploadedImages.length">
          <ul>
              <li v-for="image in uploadedImages" :key="image.id">
                  <img :src="image.src" alt="Uploaded image">
              </li>
          </ul>
      </div>
      <div v-else>
        <p>No images uploaded yet.</p>
      </div>
    </div>
  </template>
<script>
export default {
  data () {
    return {
      uploadedImages: []
    }
  },
  created () {
    // Ambil daftar URL gambar yang berhasil diupload dari LocalStorage
    const uploadedImages = JSON.parse(localStorage.getItem('uploadedImages'))
    if (uploadedImages) {
      this.uploadedImages = uploadedImages
    }
  }
}
</script>
  <style>
  #imageGallery {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  ul {
      list-style-type: none;
      padding: 0;
  }
  li {
    display: inline-block;
    margin: 10px;
  }
  img {
    max-width: 200px;
  }
  </style>
