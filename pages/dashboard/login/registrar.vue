<template>
<div class="container">
  <div class="col-md-6 offset-md-3">
    <div class="card-mt-4">
      <div class="card-header">
        <p class="mb-0">
          Registro
        </p>
      </div>
       <b-alert v-if="success" show variant="success">Cadastrado com Sucesso</b-alert>
          <!-- {{errors}} -->
        <div class="card-body">
          <form @submit.prevent="register">
            <div class="form-group">
              <label for="">Nome</label>
              <input class="form-control" type="text" v-model="form.name" :class="{ 'is-invalid': errors.name }" placeholder="Digite seu nome">
              <div class="invalid-feedback" v-if="errors.name">
                  {{errors.name[0]}}
              </div>
            </div>
            <div class="form-group">
              <label for="">E-mail</label>
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
              <input class="btn btn-default w-100" type="submit" value="Registrar">
            </div>
          </form>
        </div>
    </div>
  </div>
</div>

</template>

<script>
export default{
  // middleware: 'auth',
  data(){
    return{
      success: false,
      form: {
        name: '',
        email: '',
        password: ''
      }
    }
  },
  methods: {
    async register(){
      try {
          await this.$axios.post('/auth/register', this.form);

          this.success = true;
          this.form.name = '';
          this.form.email = '';
          this.form.password = '';
          // this.$auth.login({data: this.form});

          // this.$router.push({name: 'index'});

      } catch(e) {
          return;
      }
    }
  }
}
</script>
