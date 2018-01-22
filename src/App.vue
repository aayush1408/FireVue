<template>
  <div id="app">
    <form>
      <input type="text" v-model="title" >
      <input type="text" v-model="body"  >
    </form>
  </div>
</template>

<script>

import db from './components/firebaseInit.js';
export default {
  name: 'App',
  data(){
    return{
      title:'',
      body:''
    }
  },
  created(){
    var self = this;
    var oldTitle = '';
    var oldBody = '';
    setInterval(()=>{
      if(self.title !== oldTitle && self.body !== oldBody ){
             db.collection("text").add({
              title: self.title,
              body:self.body
            })
            .then(function(docRef) {
              console.log("Document written with ID: ", docRef.id);
            })
            .catch(function(error) {
              console.error("Error adding document: ", error);
            });
          oldBody = self.body;
          oldTitle = self.title;
      }
    },5000)
  }   
}
</script>


