<template>

         <coinmodal v-if="modalShow" @closemodal="modalShow = false" :coin_id="this.coinmModelId"/>

    <input type="text" class="search" v-model="search" placeholder="search"/>

    <ol>
     <table class="center">
         <tr>
          <th>#</th>
          <th>Coin</th>
          <th>Price</th>
          <th>Price Chane</th>
          <th>24 Volume</th>
          </tr>
           <tr v-for="coin in filteredList" :key="coin.id">
            <td id="count"> <li> </li> </td>
            <td> <a href="" @click.prevent="show_modal(coin.id)"><img :src="coin.image"/> {{coin.name}}  {{coin.symbol}}</a> </td> 
            <td> {{coin.total_volume}}€ </td> 
            <div v-if="coin.market_cap_change_percentage_24h<0 && coin.market_cap_change_percentage_24h!==null">
            <td style="color:red"> {{coin.market_cap_change_percentage_24h}}% </td> 
            </div>
            <div v-if="coin.market_cap_change_percentage_24h>0 && coin.market_cap_change_percentage_24h!==null">
            <td style="color:green"> {{coin.market_cap_change_percentage_24h}}% </td> 
            </div>
            <div v-if="coin.market_cap_change_percentage_24h==0">
            <td style="color:black"> {{coin.market_cap_change_percentage_24h}}% </td> 
            </div>
            <div v-if="coin.market_cap_change_percentage_24h===null">
            <td style="color:black"> ------------- </td> 
            </div>
            <td> {{coin.current_price}}€ </td> 
          </tr>
     </table>
    </ol>

         <button v-on:click="previous_page">Previous page</button> 
         <input type="text" placeholder="With page?" v-on:keyup.enter="go_page" v-model="witch_page">
         Current page is {{this.page}}
         <button v-on:click="next_page">Next page</button>
</template>

<script>
import axios from 'axios'
import Coinmodal from "@/components/coinmodal.vue";

export default {
components: {
        Coinmodal
    },
data(){

return{
count: 0,
coins: '',
search: '',
page: 1,
witch_page: '',
modalShow: false,
}

},
            created: function() {
            this.loaddata();
            },
            methods: {
                loaddata: function(){
                  var vueapp = this;
                    axios.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page='+this.page+'&sparkline=false').then(function (response){
                        vueapp.coins = response.data;
                    })
                },
                next_page: function(){
                    this.page+=1;
                    this.loaddata();
                },
                previous_page: function(){
                  if(this.page>1){
                    this.page-=1;
                    this.loaddata();
                  }
                  else
                  this.loaddata();
                },
                go_page: function(){
                    this.page=this.witch_page;
                    this.loaddata();
                },
                show_modal: function(coin_id){
                    this.modalShow=true;
                    this.coinmModelId = coin_id;
                },
                increment () {
                this.count += 1;
              },
            },

  computed:{
    filteredList: function(){
      return this.coins.filter((coin) => {
        return coin.name.toLowerCase().match(this.search.toLowerCase()) ||
                coin.symbol.toLowerCase().match(this.search.toLowerCase());
      });
    }
  
}
}
</script>


<style scoped>
.modal::-webkit-scrollbar {
    width: 10px;

}

.modal::-webkit-scrollbar-track {
    background-color: darkgrey;
}

.modal::-webkit-scrollbar-thumb {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}




td {
  border-bottom: 1px solid #ddd;
}

th{
    border-bottom: 2px solid black;
    margin:0px;
}

img{
  height:5.5%;
  width:5.5%;
}

button{
  margin:40px;
}
li{
  position: relative;
  margin-left: 15px;
  text-decoration-line: none;
}
.center {
  margin-left: auto;
  margin-right: auto;
  width: 50%;

}

.modal{
  position: fixed;
  top: 10%;
  width:40%;
  height: 80%;
  background-color: antiquewhite;
  border: solid black;
  margin-left: 30%;
  margin-right: 30%;
  overflow-y: scroll;
  scrollbar-color: #6969dd #e0e0e0;
  scrollbar-width: thin;
  opacity: 95%;
}

.search{
  width: 40%;
  border: 3px solid #00B4CC;
  padding: 5px;
  height: 20px;
  border-radius: 5px 5px 5px 5px;
  outline: none;
  color: #9DBFAF;
}

</style>