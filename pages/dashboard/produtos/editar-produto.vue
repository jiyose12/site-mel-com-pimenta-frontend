<template>
 <b-card>
    <nuxt ref="page"/>
    <custom-loading v-if="responseState.isLoading"></custom-loading>
    <div slot="header">
      <strong>Edição</strong> de Produtos
    </div>
    <span class="oldItems">{{oldItems.name}}</span>
    <b-form-group
      id="fieldset-1"
      label="Nome do Produto"
      label-for="input-name"
    >
      <b-form-input aria-describedby="input-name-live-feedback" required size='lg' id="input-name" v-model="items.name" trim></b-form-input>
      <b-form-invalid-feedback id="input-name-live-feedback">
        Este campo é obrigatório e com no mínimo 3 letras.
      </b-form-invalid-feedback>
    </b-form-group>
    <b-row>
      <b-col>
        <span class="oldItems">{{oldItems.category}}</span>
        <b-form-group
        label-for="category"
        label="Selecione a Categoria"
        >
          <b-form-select
          @change.native="fetchSubcategoryOptions($event)"
          id="category"
          size='lg'
          v-model="categorySelected"
          :options="categoryOptions">
          </b-form-select>
          <b-form-invalid-feedback>
            Este campo é obrigatório
          </b-form-invalid-feedback>
        </b-form-group>
      </b-col>
      <b-col>
        <span class="oldItems">{{oldItems.subcategory}}</span>
        <b-form-group
        label-for="subcategory"
        label="Selecione a Subcategoria"
        >
          <b-form-select
          id="subcategory"
          size='lg'
          v-model="subcategorySelected"
          :options="subcategoryOptions">
          </b-form-select>
          <b-form-invalid-feedback>
            Este campo é obrigatório
          </b-form-invalid-feedback>
        </b-form-group>
      </b-col>
    </b-row>

    <b-row>
      <b-col>
        <span class="oldItems">{{oldItems.amount}}</span>
        <b-form-group
        label-for="amount"
        label="Informe a Quantidade"
        description="Quantidade em estoque."
        >
          <b-form-input
          id="amount" size='lg'
          type="number"
          v-model="items.amount"
          placeholder="Quantidade">
          </b-form-input>
          <b-form-invalid-feedback>
            Este campo é obrigatório
          </b-form-invalid-feedback>
        </b-form-group>
      </b-col>
      <b-col>
        <span class="oldItems">{{oldItems.gross_price}}</span>
        <b-form-group
        label-for="gross-price"
        label="Informe o Preço Bruto"
        description="Preço bruto do produto."
        >
          <b-form-input
          id="gross-price"
          size='lg'
          type="number"
          v-model="items.gross_price"
          placeholder="Preço Bruto">
          </b-form-input>
          <b-form-invalid-feedback>
            Este campo é obrigatório
          </b-form-invalid-feedback>
        </b-form-group>
      </b-col>
      <b-col>
        <span class="oldItems">{{oldItems.discount}}</span>
        <b-form-group
        label-for="discount"
        label="Informe o Desconto"
        description="Caso haja desconto, gerará automaticamente o preço líquido do produto."
        >
          <b-form-input id="discount" size='lg' type="number" v-model="items.discount" placeholder="Desconto"></b-form-input>
        </b-form-group>
      </b-col>
    </b-row>

    <span class="oldItems">{{oldItems.size}}</span>
    <b-form-group
    v-if="this.categorySelected == 'Moda Íntima'"
    label-for="checkbox-group-1"
    label="Informe os Tamanhos"
    >
      <b-form-checkbox-group
        id="checkbox-group-1"
        v-model="sizeSelected"
        :options="sizeOptions"
        name="size"
      ></b-form-checkbox-group>
      <div class="mt-3">Selecionados: <strong>{{ sizeSelected }}</strong></div>
    </b-form-group>

    <span class="oldItems">{{oldItems.flavor}}</span>
    <b-form-group
    v-if="this.categorySelected == 'Sex Shop'"
    label-for="checkbox-group-2"
    label="Informe os Sabores"
    >
      <b-form-checkbox-group
        id="checkbox-group-2"
        v-model="flavorSelected"
        :options="flavorOptions"
        name="flavor"
      ></b-form-checkbox-group>
      <div class="mt-3">Selecionados: <strong>{{ flavorSelected }}</strong></div>
    </b-form-group>

    <span class="oldItems">{{oldItems.color}}</span>
    <b-form-group
    v-if="this.categorySelected == 'Moda Íntima'"
    label-for="color"
    label="Informe as Cores"
    >
      <no-ssr>
        <vue-tags-input
          id="color"
          v-model="items.color"
          :tags="colors"
          @tags-changed="newTags => colors = newTags"
        />
      </no-ssr>
      <div class="mt-3">Cores: <strong>{{ colors }}</strong></div>
    </b-form-group>

    <span class="oldItems">{{oldItems.description}}</span>
    <b-form-group
    label-for="textarea"
    label="Descrição do Produto"
    >
      <b-form-textarea
        size='lg'
        id="textarea"
        v-model="items.description"
        placeholder="Descrição..."
        rows="3"
        max-rows="6"
      ></b-form-textarea>
      <b-form-invalid-feedback>
        Este campo é obrigatório
      </b-form-invalid-feedback>
    </b-form-group>

    <b-form-group>
      <b-form-file
      size='lg'
      v-model="file"
      :state="Boolean(file)"
      placeholder="Escolha o Arquivo ou Arraste-o até Aqui"
      drop-placeholder="Arraste o arquivo até Aqui..."
      @change.native="onFileSelected($event)"
      ></b-form-file>
    </b-form-group>

    <div slot="footer">
      <b-button @click="submitEdit" type="submit" size="lg" variant="primary"><i class="fa fa-dot-circle-o"></i> Submeter</b-button>
      <b-button @click="resetFields" type="reset" size="md" variant="danger"><i class="fa fa-ban"></i> Resetar</b-button>
    </div>
  </b-card>
</template>


 <script>

  // import { required, minLength } from 'vuelidate/lib/validators'
  export default {
    middleware: 'auth',
    computed: {

    },
    data() {
      return {
        errors: [],
        responseState: {
          message: '',
          error: '',
          isLoading: false
        },
        oldItems: [],
        colorsSelected: [],
        categorySelected: null,
        responseImage: '',
        colors: [],
        file: null,
        category: '',
        subcategory: '',
        items: {
          image_product: '',
          color: '',
          flavor: '',
          size: '',
          name: '',
          amount: null,
          gross_price: null,
          discount: null,
          description: '',
          subcategory_id: '',
          category_id: ''
        },
        categoryOptions: [
          { value: null, text: 'Selecione uma opção de Categoria' },
          { value: 'Moda Íntima', text: 'Moda Íntima' },
          { value: 'Sex Shop', text: 'Sex Shop' }
        ],
        subcategoryOptions: [],
        subcategorySelected: null,
        sizeSelected: [], // Must be an array reference!
        sizeOptions: [
          { text: 'P', value: 'P' },
          { text: 'M', value: 'M' },
          { text: 'G', value: 'G' },
          { text: 'GG', value: 'GG' },
          { text: '38', value: '38' },
          { text: '39', value: '39' },
          { text: '40', value: '40' },
          { text: '41', value: '41' },
          { text: '42', value: '42' },
          { text: '43', value: '43' },
          { text: '44', value: '44' },
          { text: '45', value: '45' },
          { text: '46', value: '46' },
          { text: '47', value: '47' },
          { text: '48', value: '48' },
          { text: '49', value: '49' },
          { text: '50', value: '50' }
        ],
        flavorSelected: [], // Must be an array reference!
        flavorOptions: [
          { text: 'Morango', value: 'Morango' },
          { text: 'Chocolate', value: 'Chocolate' },
          { text: 'Tutti frutti', value: 'Tutti frutti' },
          { text: 'Uva', value: 'Uva' },
          { text: 'Melancia', value: 'Melancia' },
          { text: 'Menta', value: 'Menta' }
        ]
      }
    },
    validations: {
      // name: { minLength: minLength(3) },
      // amount: {  },
      // grossprice: {  },
      // file: {  },
      // text: {  },
      // categorySelected: {  },
      // subcategorySelected: {  }
      // // rules object
    },
    created () {
       let id = this.$route.query.id
    //   this.$axios.get(`/product/` +id + '/edit')
    //   .then(response => {
    //     this.posts = response.data
    //   })
    //   .catch(e => {
    //     this.errors.push(e)
    //   })

        this.$axios.get(`/product/`+id)
          .then(response => {
            this.oldItems = response.data
          })
          .catch(e => {
            this.errors.push(e)
          })
      },
  methods: {
    async submitEdit () {
        if (this.responseState.isLoading) {
          return false
        }
        this.responseState.isLoading = true

        /* File upload */
        if(this.file !== null){
          const fd = new FormData();
          fd.append('image_product', this.file, this.file.name)
          try {
            const responseImage  = await this.$axios.post('/product/saveimage',
              fd,
              {
              headers: {
                'Content-Type': 'multipart/form-data'
              }
            })

            this.items.image_product = responseImage.data.fileNameToStore
          } catch (e) {
            this.responseState = { error: e, errorText: 'Não foi possível enviar a imagem', isLoading: false }
          }
        }
        /* Tratamento de variaveis */
        for (const color of this.colors) {
          this.colorsSelected.push(color.text)
        }
        this.items.color = this.colorsSelected.sort().join(),
        this.items.size = this.sizeSelected.sort().join(),
        this.items.flavor = this.flavorSelected.sort().join()

      let id = this.$route.query.id
      this.$axios.patch(`/product/`+id,this.items)
        .then(response => {
          console.log(response)

          this.items = response.data
        })
        .catch(e => {
          this.errors.push(e)
        })
        //  try {
        //     const { data } = await this.$axios.patch(`/product/`+id,
        //     {
        //         name: this.items.name,
        //       })

        //       this.responseState.message = data.message
        //   } catch (e) {
        //     this.responseState.error = e
        //     this.responseState.isLoading = false
        //   }

        this.responseState.isLoading = false
    },
      fetchSubcategoryOptions(event){
        if(event.target.value == 'Moda Íntima'){
          this.subcategoryOptions = [
          { value: null, text: 'Selecione uma opção de Subcategoria' },
          { value: 'Calcinhas básicas', text: 'Calcinhas básicas' },
          { value: 'Calcinhas fio dental', text: 'Calcinhas fio dental' },
          { value: 'Sutiãs', text: 'Sutiãs' },
          { value: 'Conjunto lingerie', text: 'Conjunto lingerie' },
          { value: 'Camisolas', text: 'Camisolas' },
          { value: 'Baby doll', text: 'Baby doll' }
          ]
        }
        else if(event.target.value == 'Sex Shop'){
          this.subcategoryOptions = [
          { value: null, text: 'Selecione uma opção de Subcategoria' },
          { value: 'Lubrificantes', text: 'Lubrificantes' },
          { value: 'Pomadas', text: 'Pomadas' },
          { value: 'Comestíveis', text: 'Comestíveis' },
          { value: 'Brincadeiras eróticas', text: 'Brincadeiras eróticas' },
          { value: 'Vibradores', text: 'Vibradores' }
          ]
        }
        else
           this.subcategoryOptions = []
      },
      onFileSelected(event) {
        this.file = event.target.files[0];
      },
      resetFields() {
        this.name = ''
        this.color = ''
        this.colors = []
        this.colorsSelected = []
        this.amount = null
        this.grossprice = null
        this.discount = null
        this.file = null
        this.text = ''
        this.categorySelected = null
        this.subcategorySelected = null
        this.sizeSelected = []
        this.flavorSelected = []
      }
  }
}
</script>

<style>
.oldItems{
  color: red;
}
</style>
