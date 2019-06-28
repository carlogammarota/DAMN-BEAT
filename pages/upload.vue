<template>
    <div>

        <!-- <b-button @click="modalShow = !modalShow">Open Modal</b-button>
        <br>
        <b-modal v-model="modalShow">Hello From Modal!</b-modal> -->

        <!-- <input type="file" :v-model="asd" /> -->
        <!-- <input type="submit" value="Upload!"  /> -->
        <!-- <button @click="sendFile(dataUpload)" >ENVIAR</button> -->

        <input type="file" @change="processFile($event)">
        <!-- <button @click="sendFile()">send File</button> -->
    </div>
</template>

<script>
const axios = require("axios");
export default {
  data() {
    return {
      someData: "",
      modalShow: false
    };
  },
  mounted() {
    // this.getBeats();
  },
  methods: {
    async processFile(event) {
      var bodyFormData = new FormData();
      let media = event.target.files[0];
      bodyFormData.append("media", media);

      let res = await axios({
        method: "post",
        url: "https://api.cosmicjs.com/v1/damn-vorterix-1/media",
        config: { headers: { "Content-Type": "multipart/form-data" } },
        data: bodyFormData
      });

      let data = res.data.media;
      //   console.log("data", data);

      // add object
      // /v1/:your-bucket-slug/add-object
      let res2 = await axios({
        method: "post",
        url: "https://api.cosmicjs.com/v1/damn-vorterix-1/add-object",
        config: { headers: { "Content-Type": "multipart/form-data" } },
        data: {
          title: "Nuevo Titulo 1",
          slug: "new-titulo",
          type_slug: "tracks",
          content: "",
          metafields: [
            {
              type: "file",
              title: "Musica",
              key: "audio",
              value: data.name
            }
          ]
        }
      });

      let track = await axios({
        method: "get",
        url:
          "https://api.cosmicjs.com/v1/damn-vorterix-1/object/new-titulo-a4547010-993e-11e9-9880-2da09d6d4587",
        config: { headers: { "Content-Type": "multipart/form-data" } }
      });
      console.log("beats", track);
      let track2 = track.data.object;

      console.log("track2", track2);
      this.addToBeats(track2);
      //   console.log("RES2", res2.data.object);
    },
    async addToBeats(obj) {
      //https://api.cosmicjs.com/:bucket_slug/object/:slug
      let beats = await axios({
        method: "get",
        url: "https://api.cosmicjs.com/v1/damn-vorterix-1/object/walter",
        config: { headers: { "Content-Type": "multipart/form-data" } }
      });
      console.log("beats", beats);
      let beat = beats.data.object;
      //   let metafields = beats.data.object.metafields;
      //   metafields[1].push(obj);

      //   console.log("metafields 1", beat.metafields[0].objects);
      beat.metafields[0].objects.push(obj);
      beat.metadata.tracks.push(obj);
      beat.title = "Cambio";
      console.log("new beat", {
        title: "nuevo titulo",
        metafields: beat.metafields
      });
      let res = await axios({
        method: "PUT",
        url: "https://api.cosmicjs.com/v1/damn-vorterix-1/edit-object",
        config: { headers: { "Content-Type": "multipart/form-data" } },
        data: {
          slug: beat.slug,
          title: "nuevo titulo",
          metafields: beat.metafields
        }
      });

      console.log("res", res);
    }

    //  sendFile(dataUpload){

    //      console.log("this.dataUpload", dataUpload)

    // axios.post('https://api.cosmicjs.com/v1/damn-vorterix-1/media')
    //     .then(function (response) {
    //         // handle success
    //         console.log(response);
    //     })
    //     .catch(function (error) {
    //         // handle error
    //         console.log(error);
    //     })
    //     .finally(function () {
    //         // always executed
    //     });

    //     // Want to use async/await? Add the `async` keyword to your outer function/method.
    //     async function getUser() {
    //     try {
    //         const response = await axios.get('/user?ID=12345');
    //         console.log(response);
    //     } catch (error) {
    //         console.error(error);
    //     }
    // }
    // }
  }
};
</script>  
<style lang="scss">
/* // Then include the following */
@import "bootstrap/scss/bootstrap.scss";
@import "bootstrap-vue/src/index.scss";
</style>
