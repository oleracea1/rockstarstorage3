<template>
  <div class="app">
    <div class="upload-container">
      <div class="greeting-text">Choose File to upload to "IT Rockstars AWS S3 Bucket"</div>
      <input type="file" @change="uploadImageAndShow" class="file-input" />
      <div v-if="imageUrl" class="image-container">
        <img :src="imageUrl" alt="Uploaded Image" class="uploaded-image" />
      </div>
    </div>
    <div class="logo-container">
      <img src="./src/itrlogo_transp_flo.png" alt="IT Rockstars Logo" class="logo" />
    </div>
  </div>
</template>

<script>
import { Amplify } from 'aws-amplify';
import amplifyconfig from './src/amplifyconfiguration.json';
Amplify.configure(amplifyconfig);

import { uploadData } from 'aws-amplify/storage';
import { getUrl } from 'aws-amplify/storage';

export default {
  data() {
    return {
      imageUrl: null,
    };
  },
  methods: {
    async uploadImageAndShow(event) {
      if (event?.target?.files) {
        const file = event.target.files[0];
        uploadData({
          key: file.name,
          data: file
        });
        const getUrlResult = await getUrl({
          key: file.name
        });
        console.log('signed URL: ', getUrlResult.url);
        console.log('URL expires at: ', getUrlResult.expiresAt);
        this.imageUrl = getUrlResult.url;
        console.log("Picture successfully loaded from S3!")
      }
    }
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #0e75ab; /* Use blue color from the IT Rockstars logo */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.app {
  position: relative; /* Needed for the logo positioning */
}

.greeting-text {
  font-size: 20px;
  font-weight: bold; /* Make the text bolder */
  color: #333; /* Adjust color to be cooler */
  margin-bottom: 20px;
}

.upload-container {
  background-color: rgb(235, 250, 252);
  border-radius: 10px;
  padding: 40px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  text-align: center;
  animation: fadeInUp 1s ease forwards;
  position: relative; /* Needed for centering uploaded image */
}

.logo-container {
  padding: 40px;
}

.upload-container:hover {
  background-color: rgb(246, 249, 255);
}

input[type="file"] {
  border: none;
  border-radius: 5px;
  padding: 15px 20px;
  margin-bottom: 30px;
  background-color: #ffffff;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

input[type="file"]:hover {
  background-color: #f7f7f7;
}

.image-container {
  position: relative;
  width: 100%;
  height: 300px; /* Adjust height as needed */
}

.uploaded-image {
  max-width: 100%;
  max-height: 100%;
  border-radius: 10px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* Center the image */
}

.file-name {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 5px;
  padding: 5px 10px;
  font-size: 14px;
}

.logo {
  position: absolute;
  bottom: -20px;
  right: -20px;
  width: 30%; /* Adjust size as needed */
  opacity: 1.0; /* Adjust opacity to make it a watermark */
  animation: fadeInUp 3s ease forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
