<template>
        <b-card>
          <div slot="header">
            <strong>Cadastro</strong> de Produtos
          </div>

          <b-form-group
            id="fieldset-1"
            label="Nome do Produto"
            label-for="input-1"
            :invalid-feedback="invalidFeedback"
            :valid-feedback="validFeedback"
            :state="state"
          >
            <b-form-input required size='lg' id="input-1" v-model="name" :state="state" trim></b-form-input>
          </b-form-group>

          <b-row>
            <b-col>
              <b-form-group
              label-for="category"
              label="Selecione a Categoria"
              >
                <b-form-select @change.native="fetchSubcategoryOptions($event)" id="category" size='lg' v-model="categorySelected" :options="categoryOptions"></b-form-select>
              </b-form-group>
            </b-col>
            <b-col>
              <b-form-group
              label-for="subcategory"
              label="Selecione a Subcategoria"
              >
                <b-form-select id="subcategory" size='lg' v-model="subcategorySelected" :options="subcategoryOptions"></b-form-select>
              </b-form-group>
            </b-col>
          </b-row>


          <b-row>
            <b-col>
              <b-form-group
              label-for="gross-price"
              label="Informe o Preço Bruto"
              description="Preço bruto do produto."
              >
                <b-form-input id="gross-price" size='lg' type="number" v-model="grossprice" placeholder="Preço Bruto"></b-form-input>
              </b-form-group>
            </b-col>
            <b-col>
              <b-form-group
              label-for="discount"
              label="Informe o Desconto"
              description="Caso haja desconto, gerará automaticamente o preço líquido do produto."
              >
                <b-form-input id="discount" size='lg' type="number" v-model="discount" placeholder="Desconto"></b-form-input>
              </b-form-group>
            </b-col>
          </b-row>


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

          <b-form-group
          v-if="this.categorySelected == 'Moda Íntima'"
          label-for="color"
          label="Informe as Cores"
          >
            <no-ssr>
              <vue-tags-input
                id="color"
                v-model="color"
                :tags="colors"
                @tags-changed="newTags => colors = newTags"
              />
            </no-ssr>
            <div class="mt-3">Cores: <strong>{{ colors }}</strong></div>
          </b-form-group>


          <b-form-group
          label-for="textarea"
          label="Descrição do Produto"
          >
            <b-form-textarea
              size='lg'
              id="textarea"
              v-model="text"
              placeholder="Descrição..."
              rows="3"
              max-rows="6"
            ></b-form-textarea>
          </b-form-group>

          <b-form-group>
             <b-form-file
              size='lg'
              v-model="file"
              :state="Boolean(file)"
              placeholder="Choose a file or drop it here..."
              drop-placeholder="Drop file here..."
              ></b-form-file>
          </b-form-group>

          <div slot="footer">
            <b-button type="submit" size="lg" variant="primary"><i class="fa fa-dot-circle-o"></i> Submit</b-button>
            <b-button type="reset" size="md" variant="danger"><i class="fa fa-ban"></i> Reset</b-button>
          </div>
        </b-card>
</template>

<script>
  export default {
    computed: {
      state() {
        return this.name.length >= 2 ? true : false
      },
      invalidFeedback() {
        if (this.name.length > 1) {
          return ''
        } else if (this.name.length > 0) {
          return 'Precisa ter mais que 1 caractere'
        } else {
          return 'Obrigatório digitar o nome'
        }
      },
      validFeedback() {
        return this.state === true ? 'Ok' : ''
      }
    },
    data() {
      return {
        color: '',
        colors: [],
        name: '',
        grossprice: null,
        discount: null,
        file: null,
        text: '',
        size: 3,
        categorySelected: null,
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
    methods: {
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
      }
    }
  };
</script>
