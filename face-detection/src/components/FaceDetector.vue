<template>
  <v-container>
    <v-row class="text-center">

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Upload a photo below to begin!
        </h1>

        <p class="subheading font-weight-regular">
          Code for this site and the face detection service found
          <a
              href="https://github.com/Wallace99"
              target="_blank"
          >here.</a>
        </p>
      </v-col>
    </v-row>
    <v-row>
      <v-card
          class="mx-auto"
          min-width="300px">
        <v-card-actions class="justify-center">
          <v-file-input
              chips
              label="Choose a photo to detect"
              accept="image/jpeg"
              @change="upload"
          ></v-file-input>
        </v-card-actions>
        <v-card-actions class="justify-center">
          <v-btn :disabled="!isValid" @click="submitFile">
            Upload
          </v-btn>
          <v-btn :disabled="!isUploaded" @click="detect">
            Detect faces
          </v-btn>
        </v-card-actions>
        <v-card-actions class="justify-center">
          <h3>{{ numberOfFaces }} faces detected </h3>
        </v-card-actions>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
const axios = require('axios');
export default {
  name: 'FaceDetector',

  methods: {
    upload(file) {
      if (file == null) {
        this.isValid = false
        return
      }
      if (typeof file !== 'undefined') {
        this.file = file
        this.isValid = true
        console.log(this.file)
      }

    },

    async submitFile() {
      let formData = new FormData();
      formData.append('fileUpload', this.file)

      try {
        const response = await axios.post('http://localhost:8082/faces/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        if (response.status === 200) {
          this.isUploaded = true
        }
      }
      catch (err) {
        console.log(err)
      }
    },

    async detect() {
      try {
        const response = await axios.get('http://localhost:8082/faces/numFaces');
        this.numberOfFaces = response.data
      }
      catch (err) {
        this.numberOfFaces = 0
        console.log(err)
      }

    }
  },

  data: () => ({
    file: '', numberOfFaces: 0, isValid: false, isUploaded: false
  }),
}
</script>
