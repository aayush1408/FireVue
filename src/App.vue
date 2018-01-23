<template>
  <div id="app">
    <form>
      <input type="text" v-model="title" placeholder="Enter the title">
      <input type="text" v-model="body" placeholder="Enter the body">
    </form>
      <h1>Title - {{texts.title}} </h1>
      <h1>Body - {{texts.body}}</h1>
  </div>
</template>

<script>
import db from './components/firebaseInit.js';
export default {
  name: 'App',
  components:{
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
             db.collection("text").doc('texty').set({
              title: self.title,
              body:self.body
            })
            .then(function(docRef) {
               db.collection("text").get().then((querySnapshot) => {
                    querySnapshot.forEach((doc) => {
                         self.texts = doc.data();
                      });
                  });
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


