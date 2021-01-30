<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h1>Face Detection</h1>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <FaceDetector/>
    </v-main>
  </v-app>
</template>

<script>
import FaceDetector from './components/FaceDetector';

export default {
  name: 'App',

  components: {
    FaceDetector,
  },

  data: () => ({
    //
  }),
};
</script>
