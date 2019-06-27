<template>
    <div>

 
        <b-button @click="modalShow = !modalShow">Open Modal</b-button>
        <br>
        <b-modal v-model="modalShow">Hello From Modal!</b-modal>

            <!-- <input type="file" :v-model="asd" /> -->
            <!-- <input type="submit" value="Upload!"  /> -->
           <!-- <button @click="sendFile(dataUpload)" >ENVIAR</button> -->

           <input type="file" @change="processFile($event)">
            <button @click="sendFile()">send File</button>
    </div>
</template>

<script>
const axios = require('axios');
export default {
    data(){
        return {
            someData: "",
            modalShow: false
        }
    },
    methods: {

      
      
        sendFile(){
             var formData = new FormData();
            // var imagefile = document.querySelector('#file');

            formData.append("image", event.target.files[0]);



            axios.post('https://api.cosmicjs.com/v1/damn-vorterix/media', formData, {
                headers: {
                'Content-Type': 'multipart/form-data'
                }
            })     .then(function (response) {
                    // handle success
                    console.log(response);
                })
                .catch(function (error) {
                    // handle error
                    console.log(error);
                })
                .finally(function () {
                    // always executed
                });


        },
        processFile(event) {
            this.someData = event.target.files[0]
            console.log(this.someData)

        },


        //  sendFile(dataUpload){

        //      console.log("this.dataUpload", dataUpload)

            // axios.post('https://api.cosmicjs.com/v1/damn-vorterix/media')
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
<style lang="scss">
/* // Then include the following */
@import 'bootstrap/scss/bootstrap.scss';
@import 'bootstrap-vue/src/index.scss';
</style>
