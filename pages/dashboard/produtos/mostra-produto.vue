<template>
  <div >

       <router-link to="/dashboard/produtos/listar-produtos"><i class="fa fa-arrow-left">Volta</i></router-link>


      <h2>{{posts.name}}</h2>
      <h2>{{posts.gross_price}}</h2>
      <h2>{{posts.discount}}</h2>
      <img :src="'http://localhost:8000/storage/image_product/' + posts.image_product" :alt="`${posts.image_product}`">
      <hr>


  </div>
</template>
 <script>

export default {
  middleware: 'auth',
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
