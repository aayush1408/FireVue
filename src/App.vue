<template>
  <div id="app">
    <form>
      <input type="text" v-model="title" >
      <input type="text" v-model="body"  >
    </form>
    <ul v-for="text in texts">
      <li>{{text.title}} -- {{text.body}}</li>
    </ul>
  </div>
</template>

<script>
import Display from './components/display.vue';
import db from './components/firebaseInit.js';
export default {
  name: 'App',
  components:{
    Display
  },
  data(){
    return{
      title:'',
      body:'',
      texts:[]
    }
  },
  created(){
    var self = this;
    var oldTitle = '';
    var oldBody = '';
    setInterval(()=>{
      if(self.title !== oldTitle || self.body !== oldBody ){
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
  } ,  
  mounted(){
      db.collection("text").get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
           this.texts.push(doc.data());
      });
       console.log(this.texts);
  });
}
}
</script>


