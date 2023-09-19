<template>
  <div id="app">
    <h1>Image Library</h1>
    <input type="file" @change="uploadFile">
    <div v-if="uploadError">
      <p>{{ uploadError }}</p>
    </div>
    <div v-if="uploadSuccess">
      <p>Gambar berhasil diupload!</p>
      <img v-for="image in images" :src="image.url" :alt="image.name" v-bind:key="image.id">
    </div>
  </div>
</template>
<script>
import { ref } from 'vue'
export default {
  setup () {
    const images = ref([])
    const uploadError = ref(null)
    const uploadSuccess = ref(false)
    const uploadFile = (e) => {
      const file = e.target.files[0]
      // Validate file
      if (!file.type.match('image.*')) {
        uploadError.value = 'Hanya boleh file gambar'
        return
      }
      if (file.size > 2000000) {
        uploadError.value = 'File gambar terlalu besar'
        return
      }
      // Upload file to imgBB
      const formData = new FormData()
      formData.append('image', file)
      fetch('https://api.imgbb.com/1/upload', {
        method: 'POST',
        body: formData,
        headers: {
          Authorization: 'Client-ID YOUR_CLIENT_ID'
        }
      })
        .then(response => response.json())
        .then(data => {
          if (data.success) {
            images.value.push({
              url: data.data.url,
              name: data.data.name
            })
            uploadSuccess.value = true
          } else {
            uploadError.value = 'Upload error'
          }
        })
      try {
        // Code that might throw an error
      } catch (error) {
        uploadError.value = 'Upload error'
      }
    }
    return {
      images,
      uploadError,
      uploadSuccess,
      uploadFile
    }
  }
}
</script>
<style>
#app {
  font-family : 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
input[type="file"] {
  display: block;
  margin: 0 auto;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
img {
  display: block;
  margin: 0 auto;
  max-width: 100%;
}
</style>
