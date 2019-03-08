<!--  -->
<template>
      <div class="row">
          <p class="search-result" v-if="searchResult">找到约{{count}}个结果</p>
          <h3 v-if="firstview">Enter the name you search</h3>
          <h3 v-if="loading">Loaing...</h3>
          <h3 v-if="errMsg">{{errMsg}}</h3>
          <div v-else class="card" v-for="(user,index) in users" :key="index">
              <a :href="user.url" target="_blank">
                <img class="card-img" :src="user.icon" style="..."/>
              </a>
              <p class="card-text">{{user.name}}</p>
          </div>
      </div>
</template>

<script>
import PubSub from 'pubsub-js'
import axios from 'axios'
export default {
  data () {
    return {
      firstview:true,
      loading:false,
      errMsg:"",
      searchResult:false,
      count:0,
      users:[]
    };
  },
  mounted () {
    PubSub.subscribe('search',(msg,searchKey) => {
      //定义接口地址
      const url = `http://api.github.com/search/users?q=${searchKey}`
      //更新状态
      this.firstview = false
      this.loading = true
      this.errMsg = null
      this.users = null
      this.searchResult = true

      //发送请求
      axios.get(url).then(response => {
        const result = response.data
        const users = result.items.map(item => ({
          url:item.html_url,
          icon:item.avatar_url,
          name:item.login
        }))
        //请求成功
        this.loading = false
        console.log(users)
        users.length ? (this.users = users,this.count = users.length) : (this.errMsg = 'no message can get you search',this.count = users.length)
        //请求失败
      }).catch(error => {
        this.loading = false
        this.errMsg = 'repuest failure,Please try later...'
        this.count = users.length
      })
    })
  }



}

</script>
<style>
.row,.card,.card-text,.search-result{
    margin: 0;padding: 0;list-style: none;outline: none;
}
.body{
    text-align: center
}
.row{
    width: 90%;
    min-width: 1000px;
    margin: 50px auto;
    text-align: center;
    position: relative;
}
.card {
    float: left;
    width: 31%;
    padding: .75rem;
    margin-bottom: 2rem;
    border: 1px solid #efefef;
}
.card-img {
    width: 100px;
    height: 100px;
    margin-bottom: .75rem;
    border-radius: 100px;
    margin: 0 auto;
}
.card-text{
    font-size: 85%;
}
.search-result{
  text-align: left;
  position: absolute;
  top:-50px;
  font-size: 14px;
  color: grey;
}
</style>