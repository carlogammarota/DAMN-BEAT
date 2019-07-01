<template lang='pug'>
.menubar(:style='style')
  author
  section
    .title Discography
    albums
  section(v-if='author && (author.metadata.email || author.metadata.skype)')
    .title Upload Beat
    //- contacts
    <br>
    //- <b-button v-b-modal.modal-1>Launch demo modal</b-button>
    //- <b-button v-b-modal.modal-1 variant="primary">Upload Audio</b-button>
    <b-button v-b-modal.modal-multi-1 variant="primary">Upload Audio</b-button>
    <b-modal id="modal-multi-1" size="lg" title="Subir Beat" no-stacking>
      <p class="my-2">El tamaño del archivo no debe superar los 50mb, los formatos aceptados son MP3 y Wav.</p>
      //- <dropzone id="foo" ref="el" :options="options" @change="processFile($event)" :destroyDropzone="true"></dropzone>
      //- <b-button variant="outline-primary" @click="processFile($event)">Upload</b-button>
      //- <input type="file" @change="processFile($event)">
      <Upload></upload>
      //- .data
      //- <font-awesome-icon icon="fas-AlignLeft" />
      
      //- <font-awesome-icon :icon="['fas', 'spinner']" />
       <input type="file" @change="processFile($event)">
      //- <b-button v-b-modal.modal-multi-2>Open Second Modal</b-button>
    </b-modal>
</template>

<script>
import Dropzone from 'nuxt-dropzone'
import 'nuxt-dropzone/dropzone.css'

import { mapState } from 'vuex'
import Albums from '~/components/Albums.vue'
import Author from '~/components/Author.vue'
import Contacts from '~/components/Contacts.vue'
import CosmicLogo from '~/components/CosmicLogo.vue'
import Upload from "~/components/upload"
const axios = require("axios");

export default {
  data() {
    return {
      modalShow: false,
      options: {
        url: "http://httpbin.org/anything"
      },
      data: null
    };
  },
  name: 'menubar',
  components: {
    Upload,
    Albums,
    Author,
    Contacts,
    CosmicLogo,
    Dropzone
  },
  computed: {
    // ...mapGetters(['tracks']),
    ...mapState(['author']),
    style () {
      return {
        backgroundColor: process.env.keyColorBG,
        color: process.env.keyColorFG
      }
    }
  },
  mounted(){
    // const instance = this.$refs.el.dropzone
  },
  methods: {
    // async processFile(event) {
    //   console.log("event", event)
    //   var bodyFormData = new FormData();
    //   let media = event.target.files[0];
    //   bodyFormData.append("media", media);

    //   let res = await axios({
    //     method: "post",
    //     url: "https://api.cosmicjs.com/v1/damnvort/media",
    //     config: { headers: { "Content-Type": "multipart/form-data" } },
    //     data: bodyFormData
    //   });

    //   let data = res.data.media;
    //   //   console.log("data", data);

    //   // add object
    //   // /v1/:your-bucket-slug/add-object
    //   let res2 = await axios({
    //     method: "post",
    //     url: "https://api.cosmicjs.com/v1/damnvort/add-object",
    //     config: { headers: { "Content-Type": "multipart/form-data" } },
    //     data: {
    //       title: "Nuevo Titulo 1",
    //       slug: "new-titulo",
    //       type_slug: "tracks",
    //       content: "",
    //       metafields: [
    //         {
    //           type: "file",
    //           title: "Musica",
    //           key: "audio",
    //           value: data.name
    //         }
    //       ]
    //     }
    //   });

    //   let track = await axios({
    //     method: "get",
    //     url:
    //       "https://api.cosmicjs.com/v1/damnvort/object/new-titulo-a4547010-993e-11e9-9880-2da09d6d4587",
    //     config: { headers: { "Content-Type": "multipart/form-data" } }
    //   });
    //   console.log("beats", track);
    //   this.data = track
    //   let track2 = track.data.object;

    //   console.log("track2", track2);
    //   this.addToBeats(track2);
    //   //   console.log("RES2", res2.data.object);
    // },
    // async addToBeats(obj) {
    //   //https://api.cosmicjs.com/:bucket_slug/object/:slug
    //   let beats = await axios({
    //     method: "get",
    //     url: "https://api.cosmicjs.com/v1/damnvort/object/walter",
    //     config: { headers: { "Content-Type": "multipart/form-data" } }
    //   });
    //   console.log("beats", beats);

    //   let beat = beats.data.object;
    //   //   let metafields = beats.data.object.metafields;
    //   //   metafields[1].push(obj);

    //   //   console.log("metafields 1", beat.metafields[0].objects);
    //   console.log("obj", obj)
    //   beat.metafields.objects.push(obj);
    //   beat.metadata.tracks.value.push(obj);
    //   beat.title = "Cambio";
    //   console.log("new beat", {
    //     title: "nuevo titulo",
    //     metafields: beat.metafields
    //   });
    //   let res = await axios({
    //     method: "PUT",
    //     url: "https://api.cosmicjs.com/v1/damnvort/edit-object",
    //     config: { headers: { "Content-Type": "multipart/form-data" } },
    //     data: {
    //       slug: beat.slug,
    //       title: "nuevo titulo",
    //       // metafields: beat.metafields
    //       // metafields: beat.metafields
    //       // metafields: beat.metafields
    //       //  metafields: [{
    //       //   key: 'album_id',
    //       //   type: 'text',
    //       //   value: this.album._id
    //       // }],
    //       // [{"object_type":"tracks","value":"5b688a165669b934036d93c0","key":"tracks","title":"Tracks","type":"objects","children":null}]
    //     }
    //   });

    //   console.log("res", res);
    // }

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
}
</script>

<style scoped lang='sass'>
.menubar
  display: flex
  flex-direction: column
  padding: 48px 24px

  section
    padding-top: 48px

    &.last
      margin-top: auto

    & > .title
      font-size: 13px
      margin-bottom: 20px
</style>
