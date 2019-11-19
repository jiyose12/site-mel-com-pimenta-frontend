<template>
<div class="container">
  <div class="col-md-6 offset-md-3">
    <div class="card-mt-4">
      <div class="card-header">
        <p class="mb-0">
          Login
        </p>
      </div>
        <div class="card-body">
          <form @submit.prevent="login">
            <div class="form-group">
              <label for="">Email</label>
              <input class="form-control" type="email" v-model="form.email" :class="{ 'is-invalid': errors.email }" placeholder="Digite seu e-mail">
              <div class="invalid-feedback" v-if="errors.email">
                  {{errors.email[0]}}
              </div>
            </div>
            <div class="form-group">
              <label for="">Senha</label>
              <input class="form-control" type="password" v-model="form.password" :class="{ 'is-invalid': errors.password }" placeholder="Digite sua senha">
              <div class="invalid-feedback" v-if="errors.password">
                  {{errors.password[0]}}
              </div>
            </div>

            <div class="form-group">
              <input class="btn btn-default w-100" type="submit" value="Logar">
            </div>
          </form>
        </div>
    </div>
  </div>
</div>

</template>

<script>
export default{
  layout: 'clean',
  middleware: 'guest',

  data(){
    return{
      form: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    async login(){
      try {
        await this.$auth.login({ data: this.form });
          this.$router.push(this.$route.query.redirect ? this.$route.query.redirect : '/dashboard');

            // this.$router.push({name: 'index'});

      } catch(e) {
          return;
      }
    }
  }
}
</script>
