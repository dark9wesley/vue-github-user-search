<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input type="text" placeholder="enter the name you search" v-model="inputValue"/>
      &nbsp;
      <button @click="getUserList">Search</button>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Search',
  data() {
    return {
      inputValue: ''
    }
  },
  methods: {
    async getUserList(){
      this.$bus.$emit('getUserList', {
        isFirst: false,
        isLoading: true,
      })
      try{
        const res = await axios.get(`https://api.github.com/search/users?q=${this.inputValue}`)
        this.$bus.$emit('getUserList', {
          isLoading: false,
          userList: res.data.items,
        })
      }catch(e){
        this.$bus.$emit('getUserList', {
          isLoading: false,
          errMsg: e.message
        })
      }
     
    }, 
  },
}
</script>
