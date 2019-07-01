<template>
    <div>

        <!-- <b-button @click="modalShow = !modalShow">Open Modal</b-button>
        <br>
        <b-modal v-model="modalShow">Hello From Modal!</b-modal> -->

        <!-- <input type="file" :v-model="asd" /> -->
        <!-- <input type="submit" value="Upload!"  /> -->
        <!-- <button @click="sendFile(dataUpload)" >ENVIAR</button> -->
        <h1>UPLOAD</h1>
        <input type="file" @change="processFile($event)">
        <span v-if="uploading">Subiendo Archivo</span>
         <!-- <b-button v-b-modal.modal-1>Launch demo modal</b-button> -->

        <!-- <b-modal id="modal-1" title="BootstrapVue">
          <p class="my-4">Hello from modal!</p>
        </b-modal> -->


          <!-- <b-alert show variant="success"><a href="#" class="alert-link">Success Alert</a></b-alert>
          <b-alert show variant="danger"><a href="#" class="alert-link">Danger Alert</a></b-alert> -->
        <!-- <button @click="sendFile()">send File</button> --> 
    </div>
</template>

<script>
const axios = require("axios");
import getBucket from '~/lib/cosmic'
export default {
  data() {
    return {
      //Cambiar este slug para cambiar el album
      slugAlbum: "beats",
      someData: "",
      modalShow: false,
      uploading: false
    };
  },
  methods: {
    async processFile(event) {
      this.uploading = true;
      const bucket = await getBucket()
      let media = event.target.files[0];
      console.log(media)
      // Subimos el archivo
        let mediaUploaded = (await bucket.addMedia({
          media: media,
          folder: 'beats',
          metadata: {
            caption: 'Detalle del Beat',
            credit: 'Walter G.'
          }
        })).media;
        console.log("Archivo Subido", mediaUploaded)
        // Creamos el track
        const params = {
          title: 'Titulo del Beat 3 ('+media.name+')',
          type_slug: 'tracks',
          content: 'Descripcion del beat',
          metafields: [
              {
                type: "file",
                title: "Musica",
                key: "audio",
                value: mediaUploaded.name
              }
            ]
        }
        let track = (await bucket.addObject(params)).object;    
        console.log("Track Agregado", track)

        // Agregamos el track al album extra-clean
        
          // Primero obtengo el album, para no pisar si ya hay tracks        
          const albumObject = (await bucket.getObject({slug: this.slugAlbum})).object;
          console.log("Album antes de actualizar", albumObject);
          let metafields = albumObject.metafields;
          if(!metafields) metafields = []
          let existMetafieldTracks = false;
          metafields.map(meta => {
            console.log("buscando", meta)
            if(meta.key == 'tracks'){
              existMetafieldTracks = true;
              console.log("Encontre el meta", meta)
              // Encontre el metafield de los tracks, agreamos el que acabamos de subir.
              let values = meta.value.split(",");
              values.push(track._id);
              meta.value = values.join(",");
              console.log("Nuevo valor de Metafield", meta.value)
              return meta;
            }
            return meta;
          })

          if(!existMetafieldTracks){
            console.log("No existe metafield")
            // No existe o esta vacio, lo agregamos
            let trackMetafield = 
              {
                title: 'Tracks',
                key: 'tracks',
                type: 'objects',
                object_type: 'tracks',
                value: track._id
              };
            metafields.push(trackMetafield)
          }
          let album = await bucket.editObject({
            slug: this.slugAlbum,
            metafields: metafields
          })
        console.log("Album Actualizado", album)
        this.uploading = false;
      
    }
  }
};
</script>  
<style lang="scss">
/* // Then include the following */
@import "bootstrap/scss/bootstrap.scss";
@import "bootstrap-vue/src/index.scss";
</style>
