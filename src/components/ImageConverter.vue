<template>
  <v-container>
    <v-row>
      <v-col>
        <img :src="previewImage" class="uploading-image" style="height: 350px;width: 350px;" />
        <input type="file" accept="image/jpeg" @change=uploadImage>
        <br />
        <br />
        <v-btn color="primary" @click="backremove()">Convert</v-btn>
      </v-col>

      <v-col>
        <img :src="'data:image/png;base64,' + resultImage" class="uploading-image"
          style="height: 350px;width: 350px;" />
          <v-btn color="red" @click="downloadImage()" download :href="'data:image/png;base64,' + resultImage">Download</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  name: 'ImageConverter',

  data() {
    return {
      previewImage: null,
      resultImage: '',
    }
  },
  methods: {
    uploadImage(e) {
      const image = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = e => {
        this.previewImage = e.target.result;
        console.log(this.previewImage);
      };
    },
 
    backremove() {
      const FormData = require('form-data');
      const formData = new FormData();
      formData.append('size', 'auto');
      formData.append('image_file_b64', this.previewImage);

      axios({
        method: 'post',
        url: 'https://api.remove.bg/v1.0/removebg',
        data: formData,
        responseType: 'arraybuffer',
        headers: {
          'X-Api-Key': 'Yu8dGS2xtCijLT2bsZ2YqkT4',
        },
        encoding: null
      })
        .then((response) => {
          if (response.status != 200) return console.error('Error:', response.status, response.statusText);
          console.log(response);

          console.log(response.data + "response data");

          var base64 = btoa(
            new Uint8Array(response.data).reduce(
              (data, byte) => data + String.fromCharCode(byte),
              ""
            )
          );
          this.resultImage = base64;


        })
        .catch((error) => {
          return console.error('Request failed:', error);
        });
    }

  }
}
</script>
