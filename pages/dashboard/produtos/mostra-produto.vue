<template>
  <div >
   
       <router-link to="/dashboard/produtos/listar-produtos"><i class="fa fa-arrow-left">Volta</i></router-link>
    
    
      <h2>{{posts.name}}</h2>
      <hr>
     
    
  </div>
</template>
 <script> 
 
export default {
  data() {
    return {
      posts: [],
      errors: []
    }
  }, 
   created () {
       let id = this.$route.query.id; 
    this.$axios.get(`/product/`+id)
      .then(response => {
        this.posts = response.data
      })
      .catch(e => {
        this.errors.push(e)
      })
  }
}
</script>