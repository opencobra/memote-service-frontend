<template>
  <div class="container center-align center-box">
    <div class="form-group files">
      <label class="label-upload">Upload Your Model</label>
      <input
        type="file"
        ref="modelInput"
        class="form-control"
        accept="text/xml,.json,.xml.gz,.xml.bz2"
        @change="uploadFile()"
      >
    </div>
    <div class="progress" v-if="uploading">
      <div class="indeterminate"></div>
    </div>
  </div>
</template>

<script>
import * as axios from 'axios';

export default {
  name: 'Upload',
  data: () => ({
    uploading: false,
  }),
  methods: {
    uploadFile() {
      if(!this.$refs.modelInput.files) {
        return;
      }

      this.uploading = true;
      const formData = new FormData();
      formData.append("model", this.$refs.modelInput.files[0]);

      axios
        // TODO
        .post(`http://localhost:8000/memote-webservice/submit`, formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        }).then(response => {
          this.$store.dispatch('addTask', response.data.uuid)
        }).catch(error => {
          // TODO
          console.log(error);
        }).then(() => {
          this.$refs.modelInput.value = '';
          this.uploading = false;
        });
    },
  },
}
</script>

<style scoped>
h1 {
  font-size: 24px;
  font-weight: bold;
}

.label-upload {
  font-size: 24px;
  font-weight: bold;
  color: rgba(0,0,0,0.87);
}

.files input {
  outline: 2px dashed #2a7bb8;
  outline-offset: -10px;
  -webkit-transition: outline-offset .15s ease-in-out, background-color .15s linear;
  transition: outline-offset .15s ease-in-out, background-color .15s linear;
  padding: 120px 0px 85px 20%;
  text-align: center !important;
  width: 100% !important;
  box-sizing:border-box;
  -moz-box-sizing:border-box; /* Firefox */
  -webkit-box-sizing:border-box; /* Safari */
  margin-left:auto;
  margin-right: auto;
  cursor:pointer;
  max-width: 800px;
  display: block;
}
.files input:focus{
  outline: 2px dashed #2a7bb8;
  outline-offset: -10px;
  -webkit-transition: outline-offset .15s ease-in-out, background-color .15s linear;
  transition: outline-offset .15s ease-in-out, background-color .15s linear; border:1px solid #2a7bb8;
}
.files{
  position:relative;
  outline: 0;
}
.files:after {
  pointer-events: none;
  position: absolute;
  top: 90px;
  left: 0;
  width: 70px;
  right: 0;
  height: 70px;
  content: "";
  background-image: url(../assets/images/upload.png);
  display: block;
  margin: 0 auto;
  background-size: 100%;
  background-repeat: no-repeat;
}
.color input{ background-color:#f1f1f1;}

.files:before {
  position: absolute;
  bottom: 10px;
  left: 0;
  pointer-events: none;
  width: 100%;
  right: 0;
  height: 57px;
  content: " or drag it here. ";
  display: block;
  margin: 0 auto;
  color: #2a7bb8;
  font-weight: 600;
  text-transform: capitalize;
  text-align: center;
}

@media only screen and (max-width : 2560px) {
  .files input {
    padding: 120px 0 85px 10%;
  }
}

@media only screen and (max-width : 1440px) {
  .files input {
    padding: 120px 0 85px 20%;
  }
}
@media only screen and (max-width : 1024px) {
  .files input {
    padding: 120px 0 85px 25%;
  }
}

@media only screen and (max-width : 979px) {
  .files input {
    padding: 120px 0 85px 30%;
  }
}

@media only screen and (max-width : 767px) {

}

@media only screen and (max-width : 480px) {
  .files input {
    padding: 120px 0 85px 20%;
  }
}

@media only screen and (max-width : 320px) {
  .files input {
    padding: 120px 0 85px 4%;
  }
}

</style>