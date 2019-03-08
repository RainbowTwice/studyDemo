<!--  -->
<template>
  <div class="searchContainer">
      <p class="searchItem">{{searchItem}}</p>
      <input class="searchText" type="text" placeholder="enter the name you search" v-model="searchKey" ><input class="seachButton" type="button" @click="search"  value="search">
  </div>
</template>

<script>
import PubSub from 'pubsub-js'
export default {
  data () {
    return {
        searchItem:'Search Github Users',
        searchKey:''
    };
  },
  methods:{
      search () {
          const searchKey = this.searchKey.trim()
          if (searchKey) {
              PubSub.publish('search',searchKey)
          }
      }
  }
}

</script>
<style>
    .searchContainer,.searchItem,.searchText,.searchButton{
        margin: 0;padding: 0;list-style: none;outline: none;
    }
    .body{
        text-align: center
    }
    .searchContainer{
        width: 90%;
        min-width:1000px;
        height: 150px;
        background-color: gainsboro;
        margin: 0 auto;
    }
    .searchItem{
        font-size: 24px;
        font-weight: 500;
        padding-left: 50px;
        padding-top: 68px;
    }
    .searchText{
        width: 160px;
        height: 20px;
        padding: 0 5px;
        border: 1px solid yellowgreen;
        margin:5px 3px 0 50px;
    }
    .searchButton{
        border: 1px solid black;
    }
</style>