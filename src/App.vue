<template>
  <div id="app">
    <template v-for="path in images">
      <CloudImage :key="path" :path='`folder/${path}`'/>
    </template>
    <input type="file" ref ="myfile">
    <button @click="upload">upload</button>
  </div>
</template>

<script>
import { storage } from './firebase'
import { ref,uploadBytes } from 'firebase/storage'
import CloudImage from './components/CloudImage.vue'

export default {
  name: 'App',
  components: {
    CloudImage
  },
  data() {
    return {
      images: []
    }
  },
  methods: {
    upload: function() {
      const file = this.$refs.myfile.files[0]
      const fileName = `TripImg0${this.images.length + 1}.jpg`    
      const storageRef = ref(storage, `folder/${fileName}`)
      uploadBytes(storageRef, file).then((snapshot) => {
        console.log(snapshot)
        // Add the uploaded image to the images array
        this.images.push(fileName)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
