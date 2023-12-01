<template>
  <div class="container">
    <div class="restaurantsHeader text-center mt-5">
    
      <h1>Search Restaurants</h1>
      
      <Autocomplete 
      :historySearchList="historySearchList"
      :inputSearch="inputSearch"
      @input-change="handleInputChange"
      @item-selected="handleItemSelected" />
      <!-- <input type="text" class="form-control text-center border-1" v-model="inputSearch"> -->
      <button class="btn btn-outline-primary mt-3 col-4" @click="searchRestaurants()">Search</button>
    </div>
    <div class="listBody mt-5">
      <div class="list-head">
        <h4>Restaurants List :</h4>
      </div>
      <div class="showListRestaurants">
            <div v-if="isLoading">
                <Loading title="Loading..."/>
            </div>
            <div v-else>
                <div v-if="resList.length == 0">
                  <div class="card">
                    <div class="card-body text-center">
                      <span class="fw-bold">Not found restaurants</span>
                    </div>
                  </div>
                </div>

                <div v-else class="card mb-2"  v-for="(restnt, index) in resList" :key="index">
                  <div class="card-body">
                    <p class="fw-bold">{{ restnt.name }}</p>
                    <span>Location : <span>{{ restnt.formatted_address }}}</span></span>
                    <br/>
                    <div v-html="generateStars(restnt.rating)"></div>
                  </div>
                </div>
            </div>

      </div>
    </div>
  </div>
</template>

<script setup>
useHead({
  title: 'Restaurants Apps',
})
</script>

<script>
import Autocomplete from '@/components/Autocomplete.vue';
import axios from 'axios';
    export default {
        name:'restaurants',
        components: {
          Autocomplete,
        },
        data(){
          return{
              resList:{},
              isLoading:true,
              inputSearch:'Bang Sue',
              historySearchList: [],
          }
        },
        mounted(){
          this.searchDefault();
        },
        methods:{
            handleInputChange(searchQuery) {
              this.inputSearch = searchQuery;
            },
            handleItemSelected(item) {
              this.inputSearch = item;
            },
            generateStars(rating) {
              const stars = Array.from({ length: rating }, () => '<span class="star text-warning" data-value="1">&#9733;<span class="star-notification"></span></span>');
              return stars.join('');
            },

            searchDefault(){
              this.searchRestaurants();
            },

            searchRestaurants(){
              this.isLoading = true;
              axios.get(`http://localhost:8000/api/restaurants/search/${this.inputSearch}`).then(res => {
                this.resList = res.data.result;
                this.historySearchList = res.data.history;
                this.isLoading = false;
              })
              .catch(function (error){
                  console.log('response error. message is : ' , error);
                  myThis.isLoading= false;
                  myThis.isLoadingTitle= "Loading";
              });
            }
        }
    }
</script>

<style>
</style>