<template>
  <div class="modal">
    
    <ul>
      
    <li><button class="close" @click.prevent="close_model">X</button></li>
    <li>{{coin.name}}</li>
    <li>{{coin.symbol.toUpperCase()}}</li>
    <li><img :src="coin.image.small"/></li>
    <li>{{coin.localization.en}}</li>
    <li>{{coin.genesis_date}}</li>
    <li><span v-html="coin.description.en"></span></li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
  export default {
   props: [
     'coin_id',
   ],
   data(){

    return{
    coin: '',
    search:'',

    }

    },

        created: function() {
            this.loaddata();
            },
            methods: {
              loaddata: function(){
                var self = this;
                  axios.get('https://api.coingecko.com/api/v3/coins/'+this.coin_id).then(function (response){
                      self.coin = response.data;
                      console.log(response.data);

                  })
              },

              close_model: function(){
                this.$emit('closemodal');
              }
  },

 

     
}
</script>

<style scoped>

img{
  height:3%;
  width:3%;
}


div {
  width: 200px;
  margin: 30px;
}

h2 {
  font: 400 40px/1.5 Helvetica, Verdana, sans-serif;
  margin: 0;
  padding: 0;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

li {
  font: 200 20px/1.5 Helvetica, Verdana, sans-serif;
  border-bottom: 1px solid #ccc;
}

li:last-child {
  border: none;
  font-size: 10;
}

li a {
  text-decoration: none;
  color: #000;

  -webkit-transition: font-size 0.3s ease, background-color 0.3s ease;
  -moz-transition: font-size 0.3s ease, background-color 0.3s ease;
  -o-transition: font-size 0.3s ease, background-color 0.3s ease;
  -ms-transition: font-size 0.3s ease, background-color 0.3s ease;
  transition: font-size 0.3s ease, background-color 0.3s ease;
  display: block;
  width: 200px;
}

.close{
  position: fixed;
  display: block;
  width: 26px;
  border: none;
  background-color: #d21404;
  color:white;
  font-size: 16px;
  cursor: pointer;
  text-align: center;
}
</style>